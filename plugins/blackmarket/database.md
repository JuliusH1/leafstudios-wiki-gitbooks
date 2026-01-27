# 💾 Database

Complete guide to configuring SQLite and MySQL databases for BlackMarket.

### Overview

BlackMarket supports two database systems:

* **SQLite** - File-based, no setup required (default)
* **MySQL** - Server-based, better for large servers/networks

***

### SQLite Configuration

#### Overview

SQLite is the default database system. It's:

* ✅ Zero configuration required
* ✅ Perfect for single servers
* ✅ Automatic backups possible
* ✅ No external dependencies
* ⚠️ Not ideal for cross-server networks
* ⚠️ Lower performance on very large datasets

#### Configuration

```yaml
# config.yml
database:
  type: "sqlite"
```

#### File Location

```
plugins/BlackMarket/database.db
```

#### Backing Up SQLite

**Manual Backup**:

```bash
# Stop server first
stop

# Copy database file
cp plugins/BlackMarket/database.db backups/database-$(date +%Y%m%d).db

# Start server
start
```

**Automated Backup Script** (Linux):

```bash
#!/bin/bash
# backup-blackmarket.sh

DATE=$(date +%Y%m%d-%H%M%S)
SRC="plugins/BlackMarket/database.db"
DEST="backups/blackmarket-$DATE.db"

# Create backups directory
mkdir -p backups

# Copy database
cp "$SRC" "$DEST"

# Keep only last 7 backups
ls -t backups/blackmarket-*.db | tail -n +8 | xargs rm -f

echo "Backup created: $DEST"
```

**Add to crontab**:

```bash
# Backup daily at 3 AM
0 3 * * * /path/to/backup-blackmarket.sh
```

***

### MySQL Configuration

#### Overview

MySQL is recommended for:

* ✅ Large servers (1000+ players)
* ✅ Cross-server networks (with Redis)
* ✅ Better performance at scale
* ✅ Advanced backup options
* ⚠️ Requires external MySQL server
* ⚠️ More complex setup

#### Prerequisites

1. **MySQL Server** (5.7+) or **MariaDB** (10.2+)
2. **Database** created for BlackMarket
3. **User** with permissions

#### Step 1: Create Database

Connect to MySQL:

```bash
mysql -u root -p
```

Create database and user:

```sql
-- Create database
CREATE DATABASE blackmarket;

-- Create user (replace password!)
CREATE USER 'blackmarket'@'localhost' IDENTIFIED BY 'your_secure_password';

-- Grant permissions
GRANT ALL PRIVILEGES ON blackmarket.* TO 'blackmarket'@'localhost';

-- Apply changes
FLUSH PRIVILEGES;

-- Exit
EXIT;
```

#### Step 2: Configure Plugin

```yaml
# config.yml
database:
  type: "mysql"
  
  mysql:
    host: "localhost"          # MySQL server address
    port: 3306                 # MySQL port (usually 3306)
    database: "blackmarket"    # Database name
    username: "blackmarket"    # MySQL username
    password: "your_secure_password"  # MySQL password
    use-ssl: false            # Enable SSL connection
```

#### Step 3: Restart Server

```
/stop
# Start server
```

The plugin will automatically create all required tables on first connection.

***

### MySQL Advanced Configuration

#### Remote MySQL Server

If MySQL is on a different server:

```yaml
database:
  mysql:
    host: "192.168.1.100"  # Remote server IP
    port: 3306
    database: "blackmarket"
    username: "blackmarket"
    password: "password"
    use-ssl: false
```

**Create remote user**:

```sql
-- Allow connection from specific IP
CREATE USER 'blackmarket'@'192.168.1.50' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON blackmarket.* TO 'blackmarket'@'192.168.1.50';

-- Allow from any IP (less secure)
CREATE USER 'blackmarket'@'%' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON blackmarket.* TO 'blackmarket'@'%';

FLUSH PRIVILEGES;
```

#### SSL Connection

For secure connections:

```yaml
database:
  mysql:
    host: "mysql.example.com"
    port: 3306
    database: "blackmarket"
    username: "blackmarket"
    password: "password"
    use-ssl: true  # Enable SSL
```

**MySQL must have SSL enabled and certificates configured.**

#### Connection Pooling

BlackMarket uses HikariCP for connection pooling. Default settings:

```
Maximum Pool Size: 10
Minimum Idle: 2
Connection Timeout: 30000ms (30 seconds)
Idle Timeout: 600000ms (10 minutes)
Max Lifetime: 1800000ms (30 minutes)
```

These are optimized for most servers and cannot be changed without code modification.

***

### Database Migration

#### SQLite to MySQL

**Step 1: Check current data**:

```
/bma info
```

**Step 2: Run migration**:

```
/bma migrate mysql
```

**Step 3: Confirm migration**:

```
/bma migrate mysql confirm
```

**Step 4: Wait for completion**:

```
[BlackMarket] ════════════════════════════════════════════════
[BlackMarket]   Starting Database Migration
[BlackMarket]   From: SQLiteDatabase
[BlackMarket]   To: MySQLDatabase
[BlackMarket] ════════════════════════════════════════════════
[BlackMarket] Step 1/6: Migrating player data...
[BlackMarket]   ✓ Migrated 1250 player data records
[BlackMarket] Step 2/6: Migrating player purchases...
[BlackMarket]   ✓ Migrated 3540 purchase records
[BlackMarket] Step 3/6: Migrating player market items...
[BlackMarket]   ✓ Migrated 625 player market items
[BlackMarket] Step 4/6: Migrating global market items...
[BlackMarket]   ✓ Migrated 5 global market items
[BlackMarket] Step 5/6: Migrating item sold counts...
[BlackMarket]   ✓ Migrated 8 item sold counts
[BlackMarket] Step 6/6: Migrating purchase history...
[BlackMarket]   ✓ Migrated 2100 purchase history entries
[BlackMarket] Migrating global settings...
[BlackMarket]   ✓ Migrated global settings
[BlackMarket] ════════════════════════════════════════════════
[BlackMarket]   ✓ Migration Completed Successfully!
[BlackMarket] ════════════════════════════════════════════════
```

**Step 5: Update config**:

```yaml
# config.yml
database:
  type: "mysql"  # Change from "sqlite"
```

**Step 6: Reload**:

```
/bma reload
```

#### MySQL to SQLite

Same process, just specify `sqlite`:

```
/bma migrate sqlite
/bma migrate sqlite confirm
```

Then update config:

```yaml
database:
  type: "sqlite"
```

#### Migration Safety

✅ **What's migrated**:

* Player data
* Purchase history
* Player market items (per-player mode)
* Global market items
* Item sold counts
* Cooldown data
* One-time purchase flags
* Last reset times

✅ **Backup created**: Original database is not modified

⚠️ **Always backup before migration**:

```bash
# SQLite
cp plugins/BlackMarket/database.db database-backup.db

# MySQL
mysqldump -u blackmarket -p blackmarket > blackmarket-backup.sql
```

***

### Database Tables

BlackMarket creates these tables automatically:

#### `player_data`

Stores basic player information.

| Column       | Type        | Description                 |
| ------------ | ----------- | --------------------------- |
| `uuid`       | VARCHAR(36) | Player UUID (primary key)   |
| `last_reset` | BIGINT      | Last market reset timestamp |

#### `player_purchases`

Tracks player purchases and cooldowns.

| Column         | Type        | Description                |
| -------------- | ----------- | -------------------------- |
| `uuid`         | VARCHAR(36) | Player UUID                |
| `item_key`     | VARCHAR(64) | Item identifier            |
| `bought`       | BOOLEAN     | Whether player bought item |
| `cooldown_end` | BIGINT      | Cooldown end timestamp     |

**Primary Key**: `(uuid, item_key)`

#### `player_market_items`

Stores per-player market items (per-player mode only).

| Column     | Type        | Description     |
| ---------- | ----------- | --------------- |
| `uuid`     | VARCHAR(36) | Player UUID     |
| `item_key` | VARCHAR(64) | Item identifier |

**Primary Key**: `(uuid, item_key)`

#### `global_market_items`

Stores current global market items.

| Column     | Type        | Description                   |
| ---------- | ----------- | ----------------------------- |
| `item_key` | VARCHAR(64) | Item identifier (primary key) |

#### `item_sold_count`

Tracks how many times each item has been sold.

| Column       | Type        | Description                   |
| ------------ | ----------- | ----------------------------- |
| `item_key`   | VARCHAR(64) | Item identifier (primary key) |
| `sold_count` | INT         | Times sold                    |

#### `purchase_history`

Complete purchase history log.

| Column      | Type        | Description        |
| ----------- | ----------- | ------------------ |
| `id`        | INT         | Auto-increment ID  |
| `uuid`      | VARCHAR(36) | Player UUID        |
| `item_key`  | VARCHAR(64) | Item identifier    |
| `price`     | DOUBLE      | Purchase price     |
| `timestamp` | BIGINT      | Purchase timestamp |

#### `global_settings`

Stores global plugin settings.

| Column          | Type        | Description                |
| --------------- | ----------- | -------------------------- |
| `setting_key`   | VARCHAR(64) | Setting name (primary key) |
| `setting_value` | TEXT        | Setting value              |

***

### Database Maintenance

#### MySQL Optimization

**Analyze tables** (improves query performance):

```sql
USE blackmarket;
ANALYZE TABLE player_data, player_purchases, player_market_items, 
              global_market_items, item_sold_count, purchase_history, 
              global_settings;
```

**Optimize tables** (defragments and repairs):

```sql
USE blackmarket;
OPTIMIZE TABLE player_data, player_purchases, player_market_items,
               global_market_items, item_sold_count, purchase_history,
               global_settings;
```

**Add to cron** (run weekly):

```bash
# /etc/cron.weekly/optimize-blackmarket
#!/bin/bash
mysql -u blackmarket -p'password' blackmarket << EOF
ANALYZE TABLE player_data, player_purchases, player_market_items, global_market_items, item_sold_count, purchase_history, global_settings;
OPTIMIZE TABLE player_data, player_purchases, player_market_items, global_market_items, item_sold_count, purchase_history, global_settings;
EOF
```

#### SQLite Optimization

**Vacuum database** (reclaims space):

```bash
# Stop server
stop

# Optimize database
sqlite3 plugins/BlackMarket/database.db "VACUUM;"

# Start server
start
```

***

### Backup Strategies

#### SQLite Backups

**Option 1: Simple Copy**

```bash
cp plugins/BlackMarket/database.db backups/database-$(date +%Y%m%d).db
```

**Option 2: SQLite Backup Command**

```bash
sqlite3 plugins/BlackMarket/database.db ".backup backups/database-$(date +%Y%m%d).db"
```

**Option 3: Plugin Backup** (via backup plugin)

* Use plugins like SimpleBackup or AutoSaveWorld
* Include `plugins/BlackMarket/database.db` in backup

#### MySQL Backups

**Full Database Backup**:

```bash
mysqldump -u blackmarket -p blackmarket > blackmarket-backup.sql
```

**Compressed Backup**:

```bash
mysqldump -u blackmarket -p blackmarket | gzip > blackmarket-backup-$(date +%Y%m%d).sql.gz
```

**Automated Daily Backup**:

```bash
#!/bin/bash
# /etc/cron.daily/backup-blackmarket-mysql

DATE=$(date +%Y%m%d)
BACKUP_DIR="/backups/blackmarket"
MYSQL_USER="blackmarket"
MYSQL_PASS="password"
MYSQL_DB="blackmarket"

# Create backup directory
mkdir -p "$BACKUP_DIR"

# Dump database
mysqldump -u "$MYSQL_USER" -p"$MYSQL_PASS" "$MYSQL_DB" | gzip > "$BACKUP_DIR/blackmarket-$DATE.sql.gz"

# Keep only last 30 days
find "$BACKUP_DIR" -name "blackmarket-*.sql.gz" -mtime +30 -delete

echo "Backup completed: blackmarket-$DATE.sql.gz"
```

***

### Restore from Backup

#### SQLite Restore

```bash
# Stop server
stop

# Backup current database (just in case)
cp plugins/BlackMarket/database.db database.db.old

# Restore from backup
cp backups/database-20250117.db plugins/BlackMarket/database.db

# Start server
start
```

#### MySQL Restore

```bash
# Drop existing database
mysql -u blackmarket -p blackmarket -e "DROP DATABASE blackmarket; CREATE DATABASE blackmarket;"

# Restore from backup
mysql -u blackmarket -p blackmarket < blackmarket-backup.sql

# Restart server
```

***

### Troubleshooting

#### Issue 1: MySQL Connection Failed

**Error**: `Failed to connect to database!`

**Solutions**:

1. **Check credentials**:

```yaml
database:
  mysql:
    username: "correct_user"
    password: "correct_password"
```

2. **Verify MySQL is running**:

```bash
systemctl status mysql
# or
systemctl status mariadb
```

3. **Check MySQL port**:

```bash
netstat -tlnp | grep 3306
```

4. **Test connection manually**:

```bash
mysql -u blackmarket -p -h localhost blackmarket
```

5. **Check firewall** (for remote MySQL):

```bash
# Allow MySQL port
ufw allow 3306/tcp
```

#### Issue 2: Access Denied

**Error**: `Access denied for user 'blackmarket'@'localhost'`

**Solution**: Grant proper permissions

```sql
GRANT ALL PRIVILEGES ON blackmarket.* TO 'blackmarket'@'localhost';
FLUSH PRIVILEGES;
```

#### Issue 3: Too Many Connections

**Error**: `Too many connections`

**Solution**: Increase MySQL max connections

```sql
SET GLOBAL max_connections = 500;
```

Or in `my.cnf`:

```ini
[mysqld]
max_connections = 500
```

#### Issue 4: SQLite Database Locked

**Error**: `Database is locked`

**Solution**:

1. Ensure server is fully stopped
2. Check no other processes are using the file:

```bash
lsof | grep database.db
```

3. If needed, restart server

#### Issue 5: Slow Queries

**MySQL**: Enable slow query log

```sql
SET GLOBAL slow_query_log = 'ON';
SET GLOBAL long_query_time = 2;
```

**SQLite**: Consider migrating to MySQL for better performance

***

### Performance Tips

#### MySQL

1. **Use InnoDB engine** (default in BlackMarket)
2. **Optimize tables regularly**
3. **Use SSD storage**
4. **Increase buffer pool**:

```ini
# my.cnf
[mysqld]
innodb_buffer_pool_size = 1G
```

#### SQLite

1. **Use SSD storage**
2. **Regular VACUUM operations**
3. **Keep database file small**
4. **Consider MySQL for 500+ players**

***

### Database Monitoring

#### Check Database Size

**MySQL**:

```sql
SELECT 
    table_schema AS 'Database',
    ROUND(SUM(data_length + index_length) / 1024 / 1024, 2) AS 'Size (MB)'
FROM information_schema.tables
WHERE table_schema = 'blackmarket'
GROUP BY table_schema;
```

**SQLite**:

```bash
du -h plugins/BlackMarket/database.db
```

#### Check Table Sizes

**MySQL**:

```sql
SELECT 
    table_name AS 'Table',
    ROUND(((data_length + index_length) / 1024 / 1024), 2) AS 'Size (MB)'
FROM information_schema.tables
WHERE table_schema = 'blackmarket'
ORDER BY (data_length + index_length) DESC;
```

#### Monitor Connections

**MySQL**:

```sql
SHOW PROCESSLIST;
SHOW STATUS LIKE 'Threads_connected';
```

***

### Next Steps

* [Set up Redis for cross-server](redis-and-cross-server-synchronization.md)
* [Configure economy integration](economy-integration.md)
* [Learn about troubleshooting](troubleshooting-guide.md)
* [Return to main configuration](./)

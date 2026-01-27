# 🔧 Troubleshooting Guide

Solutions to common problems and errors with BlackMarket.

### Quick Diagnostics

#### Step 1: Check Plugin Status

```
/plugins
```

**BlackMarket should be GREEN**

#### Step 2: Check Plugin Info

```
/bma info
```

Verify all systems show correctly:

* Version
* Economy provider
* Database type
* Server type

#### Step 3: Enable Debug Mode

```
/bma debug
```

Check console for detailed logs.

#### Step 4: Check Logs

```
logs/latest.log
```

Look for `[BlackMarket]` entries with errors.

***

### Installation Issues

#### Plugin Won't Load

**Symptoms**:

* Plugin shows RED in `/plugins`
* Errors on server startup
* Commands don't work

**Solutions**:

1. **Check server version**:

```
/version

# BlackMarket requires 1.16.5+
```

2. **Check dependencies**:

```
/plugins

# Required: Economy plugin (Vault, PlayerPoints, etc.)
```

3. **Check for errors**:

```
# In logs/latest.log
[ERROR] Could not load 'plugins/BlackMarket.jar'
```

4. **Verify JAR file**:

```bash
# Check file isn't corrupted
ls -lh plugins/BlackMarket.jar

# Re-download if needed
```

5. **Check permissions**:

```bash
# File must be readable
chmod 644 plugins/BlackMarket.jar
```

***

### Configuration Issues

#### YAML Syntax Errors

**Symptoms**:

* Plugin won't reload
* `Failed to load configuration`
* Items not appearing

**Solutions**:

1. **Validate YAML syntax**:
   * Use [YAML Lint](https://www.yamllint.com/)
   * Paste your config
   * Fix reported errors
2. **Common mistakes**:

```yaml
# ❌ Wrong - Missing quotes
prefix: %prefix% <red>Error

# ✅ Correct - Quoted
prefix: "%prefix% <red>Error</red>"

# ❌ Wrong - Wrong indentation
items:
diamond_sword:
  price: 1000

# ✅ Correct - Proper indentation
items:
  diamond_sword:
    price: 1000

# ❌ Wrong - Tab characters
	material: "DIAMOND"  # Uses tab

# ✅ Correct - Spaces only
  material: "DIAMOND"  # Uses 2 spaces
```

3. **Reset to default**:

```bash
# Backup current config
mv config.yml config.yml.backup

# Delete items.yml to regenerate
rm items.yml

# Restart server (generates new defaults)
```

#### Config Not Updating

**Problem**: Changes don't apply after editing

**Solutions**:

1. **Reload config**:

```
/bma reload
```

2. **Check file saved**:
   * Ensure you saved the file
   * Check file timestamp
3. **Clear server cache**:

```
/stop
# Delete server cache
# Start server
```

4. **Verify correct file**:

```
plugins/BlackMarket/config.yml  # ✅
config/BlackMarket/config.yml   # ❌ Wrong location
```

***

### Economy Issues

#### No Economy Found

**Error**: `No economy plugin found!`

**Solutions**:

1. **Install economy plugin**:
   * Download Vault + EssentialsX
   * OR PlayerPoints
   * OR CoinsEngine
   * Place in plugins folder
2. **Check loaded**:

```
/plugins

# Must see: Vault, PlayerPoints, or CoinsEngine (GREEN)
```

3. **Restart server**:

```
/stop
# Start server
```

4. **Verify detection**:

```
/bma info
# Should show: Economy: [Plugin Name]
```

#### Wrong Economy Used

**Problem**: Item charges from wrong economy

**Solutions**:

1. **Specify economy type**:

```yaml
# items.yml
my_item:
  economy-type: "vault"  # Explicitly set
```

2. **Check default**:

```yaml
# config.yml
economy:
  default: "vault"  # Set your preferred default
```

3. **Verify available**:

```
/bma debug
# Check which economies are detected
```

#### Insufficient Funds Not Working

**Problem**: Can buy with $0

**Solutions**:

1. **Check price set**:

```yaml
my_item:
  price: 1000  # Must be > 0
```

2. **Verify economy plugin works**:

```
/balance
# Should show your money
```

3. **Test economy**:

```
/eco give YourName 1000
/blackmarket
# Try buying item
```

***

### Database Issues

#### Database Connection Failed

**Error**: `Failed to connect to database!`

**For SQLite**:

1. **Check file permissions**:

```bash
ls -l plugins/BlackMarket/database.db
# Should be readable/writable
```

2. **Check disk space**:

```bash
df -h
# Ensure space available
```

3. **Delete and regenerate**:

```bash
# Backup first!
cp database.db database.db.backup
rm database.db
# Restart server
```

**For MySQL**:

1. **Test connection**:

```bash
mysql -h localhost -u blackmarket -p blackmarket
```

2. **Check credentials**:

```yaml
# config.yml
database:
  mysql:
    username: "correct_username"
    password: "correct_password"
```

3. **Verify MySQL running**:

```bash
systemctl status mysql
# or
systemctl status mariadb
```

4. **Check firewall**:

```bash
telnet mysql-host 3306
```

5. **Grant permissions**:

```sql
GRANT ALL PRIVILEGES ON blackmarket.* TO 'blackmarket'@'localhost';
FLUSH PRIVILEGES;
```

#### Database Locked (SQLite)

**Error**: `Database is locked`

**Solutions**:

1. **Stop server completely**:

```
/stop
# Wait for full shutdown
```

2. **Check processes**:

```bash
lsof | grep database.db
# Kill any lingering processes
```

3. **Restart server**:

```
start
```

***

### GUI Issues

#### Items Not Showing

**Problem**: Market GUI opens but no items appear

**Solutions**:

1. **Check items configured**:

```yaml
# items.yml
items:
  diamond_sword:  # Must have at least one item
    material: "DIAMOND_SWORD"
    price: 1000
```

2. **Verify slots configured**:

```yaml
# config.yml
gui:
  market-item-slots:
    - 10
    - 11
    # ... more slots
```

3. **Check items-per-reset**:

```yaml
# config.yml
market:
  items-per-reset: 5  # Must be > 0
```

4. **Force market reset**:

```
/bma reset
```

5. **Check item chances**:

```yaml
# items.yml
diamond_sword:
  chance: 100  # 0 = never appears!
```

#### GUI Buttons Not Working

**Problem**: Clicking buttons does nothing

**Solutions**:

1. **Check action configured**:

```yaml
# config.yml
gui:
  items:
    close:
      action: "close"  # Must have action
```

2. **Verify slot correct**:

```yaml
close:
  slot: 53  # Must be valid slot (0-53)
```

3. **Test in vanilla**:
   * Remove other GUI plugins
   * Test if issue persists
4. **Check for conflicts**:

```
/plugins
# Disable other shop plugins temporarily
```

#### Wrong GUI Size

**Problem**: GUI too big/small

**Solutions**:

1. **Check size setting**:

```yaml
# config.yml
gui:
  rows: 6  # Valid: 1-6
  # OR
  size: 54  # Valid: 9, 18, 27, 36, 45, 54
```

2. **Ensure multiple of 9**:

```yaml
# ❌ Wrong
size: 50

# ✅ Correct
size: 54
```

3. **Adjust market slots**:

```yaml
# Ensure slots fit in GUI size
# For 54-slot GUI, use slots 0-53
market-item-slots:
  - 10
  - 11
  # Don't use slot 54+!
```

***

### Purchase Issues

#### Can't Buy Items

**Problem**: Clicking items does nothing

**Solutions**:

1. **Check permission**:

```
/lp user YourName permission check blackmarket.use
```

2. **Check item permission**:

```yaml
# items.yml
legendary_item:
  permission: "blackmarket.buy.legendary"
  # Give permission:
  # /lp user YourName permission set blackmarket.buy.legendary
```

3. **Check stock**:

```yaml
# items.yml
my_item:
  stock: 10  # If sold out, can't buy
```

```
# Check if sold out
/bma info
```

4. **Check cooldown**:

```yaml
my_item:
  cooldown: "1h"  # May still be on cooldown
```

5. **Check one-time**:

```yaml
my_item:
  one-time: true  # Can only buy once
```

#### Purchase But No Item

**Problem**: Money deducted but no item received

**Solutions**:

1. **Check command**:

```yaml
# items.yml
my_item:
  command: "give %player% diamond_sword 1"
  # Ensure command is correct
```

2. **Test command manually**:

```
/give YourName diamond_sword 1
# Does it work?
```

3. **Check console errors**:

```
# In logs after purchase
[BlackMarket] Error executing command: ...
```

4. **Verify command format**:

```yaml
# ✅ Correct
command: "give %player% diamond 1"

# ❌ Wrong (typo)
command: "give %plyer% diamond 1"
```

#### Infinite Purchases

**Problem**: Can buy item repeatedly without cooldown

**Solutions**:

1. **Set cooldown**:

```yaml
my_item:
  cooldown: "1h"  # Prevent spam
```

2. **Set one-time**:

```yaml
my_item:
  one-time: true  # Only once ever
```

3. **Set stock**:

```yaml
my_item:
  stock: 10  # Limited quantity
```

***

### Redis Issues

#### Redis Connection Failed

**Error**: `Failed to initialize Redis`

**Solutions**:

1. **Check Redis running**:

```bash
sudo systemctl status redis-server
```

2. **Test connection**:

```bash
redis-cli -h localhost -p 6379 -a password ping
# Should return: PONG
```

3. **Check password**:

```yaml
# config.yml
redis:
  password: "correct_password"
```

4. **Check firewall**:

```bash
telnet redis-host 6379
```

5. **Verify port**:

```yaml
redis:
  port: 6379  # Default
```

#### Cross-Server Not Syncing

**Problem**: Changes on one server don't appear on others

**Solutions**:

1. **Check unique server IDs**:

```yaml
# Each server must be different!
# Server 1
redis:
  server-id: "lobby-1"

# Server 2
redis:
  server-id: "survival-1"
```

2. **Verify all connected**:

```
# On each server
/bma info
# Should show: Redis: ENABLED
```

3. **Check same Redis instance**:

```yaml
# All servers must use SAME Redis
redis:
  host: "same-redis-host"
  port: 6379
  database: 0  # Same database number
```

4. **Enable debug**:

```
/bma debug
# Check console for Redis messages
```

***

### Message Issues

#### Colors Not Showing

**Problem**: Messages show `<red>` literally

**Solutions**:

1. **Enable MiniMessage**:

```yaml
# config.yml
messages:
  use-minimessage: true
```

2. **Reload messages**:

```
/bma reload messages
```

3. **Use legacy codes instead**:

```yaml
# messages.yml
prefix: "&6[Shop] &7»"  # Use & codes
```

#### Prefix Not Working

**Problem**: `%prefix%` shows literally in messages

**Solutions**:

1. **Quote messages**:

```yaml
# ❌ Wrong
no-permission: %prefix% <red>Error

# ✅ Correct
no-permission: "%prefix% <red>Error</red>"
```

2. **Define prefix**:

```yaml
# Must be defined in messages file
prefix: "<gold>[Shop]</gold>"
```

3. **Reload**:

```
/bma reload messages
```

#### Language File Not Loading

**Problem**: Changed language but messages stay English

**Solutions**:

1. **Check filename**:

```yaml
# config.yml
messages:
  lang: "ES_ES"  # Must match filename exactly!

# File must be:
plugins/BlackMarket/lang/ES_ES.yml
# NOT: es_es.yml or spanish.yml
```

2. **Enable localization**:

```yaml
messages:
  localization: true  # Must be enabled!
  lang: "ES_ES"
```

3. **Reload**:

```
/bma reload messages
```

***

### Performance Issues

#### Lag on Market Open

**Problem**: Server lags when opening GUI

**Solutions**:

1. **Reduce items-per-reset**:

```yaml
market:
  items-per-reset: 5  # Lower = less lag
```

2. **Disable effects**:

```yaml
# items.yml - Remove from all items
# effect: "VILLAGER_HAPPY"
```

3. **Use MySQL instead of SQLite**:

```yaml
database:
  type: "mysql"
```

4. **Optimize database**:

```sql
# MySQL
OPTIMIZE TABLE player_data, player_purchases;

# SQLite
sqlite3 database.db "VACUUM;"
```

#### Slow Database Queries

**Problem**: Delayed responses to commands

**Solutions**:

1. **Check database size**:

```sql
# MySQL
SELECT table_name, 
       ROUND(((data_length + index_length) / 1024 / 1024), 2) AS 'Size (MB)'
FROM information_schema.tables
WHERE table_schema = 'blackmarket';
```

2. **Optimize tables**:

```sql
OPTIMIZE TABLE player_data, player_purchases, purchase_history;
```

3. **Add indexes** (for large databases):

```sql
CREATE INDEX idx_uuid ON player_purchases(uuid);
CREATE INDEX idx_timestamp ON purchase_history(timestamp);
```

4. **Clean old data**:

```sql
# Delete old purchase history (keep last 30 days)
DELETE FROM purchase_history 
WHERE timestamp < UNIX_TIMESTAMP(DATE_SUB(NOW(), INTERVAL 30 DAY)) * 1000;
```

***

### Common Error Messages

#### `ClassNotFoundException`

**Error**: `java.lang.ClassNotFoundException: ...`

**Cause**: Missing dependency

**Solution**: Install required plugin (Vault, PlayerPoints, etc.)

#### `NullPointerException`

**Error**: `java.lang.NullPointerException at ...`

**Causes**:

1. Invalid item configuration
2. Missing required field
3. Corrupted database

**Solutions**:

1. Check item has all required fields
2. Validate YAML syntax
3. Regenerate database

#### `IllegalArgumentException`

**Error**: `java.lang.IllegalArgumentException: Invalid material`

**Cause**: Invalid material name

**Solution**: Fix material name

```yaml
# ❌ Wrong
material: "DIAMONDSWORD"

# ✅ Correct
material: "DIAMOND_SWORD"
```

***

### Getting Help

#### Before Asking for Support

1. ✅ Check this troubleshooting guide
2. ✅ Read the [FAQ](frequently-asked-questions.md)
3. ✅ Enable debug mode: `/bma debug`
4. ✅ Check `logs/latest.log` for errors
5. ✅ Test with minimal plugins (disable others)
6. ✅ Verify you're on latest BlackMarket version

#### Providing Information

When asking for help, include:

1. **Server information**:

```
/version
# Paste output
```

2. **Plugin version**:

```
/bma info
# Paste output
```

3. **Error logs**:

```
# From logs/latest.log
# Include full stack trace
```

4. **Configuration**:

```yaml
# Paste relevant config sections
# Remove sensitive info (passwords)
```

5. **Steps to reproduce**:

* What did you do?
* What happened?
* What did you expect?

#### Support Channels

* **Discord**: [Support Server](https://leafstudios.dev/discord)
* **BuiltByBit**: [Plugin Page](https://builtbybit.com/resources/blackmarket-endless-customization.73633/)
* **GitHub Issues**: Report bugs

***

### Next Steps

* [Check FAQ](frequently-asked-questions.md)
* [Review configuration guide](./)
* [Learn about commands](commands-and-permissions.md)

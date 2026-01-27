# 🔗 Redis & Cross-Server Synchronization

Complete guide to setting up Redis for multi-server networks (BungeeCord/Velocity).

### Overview

Redis enables BlackMarket to synchronize across multiple servers in a network:

* ✅ Shared global market across all servers
* ✅ Real-time stock updates
* ✅ Synchronized purchase tracking
* ✅ Instant market resets network-wide
* ✅ Compatible with BungeeCord and Velocity

**Without Redis**: Each server has independent markets **With Redis**: All servers share the same market state

***

### Prerequisites

#### Required

1. **Redis Server** (6.0+)
2. **Multiple Minecraft servers** on same network
3. **MySQL database** (recommended for cross-server)
4. **Network proxy** (BungeeCord or Velocity)

#### Installation

**Ubuntu/Debian**:

```bash
sudo apt update
sudo apt install redis-server
sudo systemctl enable redis-server
sudo systemctl start redis-server
```

**CentOS/RHEL**:

```bash
sudo yum install redis
sudo systemctl enable redis
sudo systemctl start redis
```

**Docker**:

```bash
docker run -d --name redis -p 6379:6379 redis:latest
```

**Windows**: Download from [Redis Windows](https://github.com/microsoftarchive/redis/releases)

***

### Basic Configuration

#### Step 1: Configure Redis Server

Edit Redis configuration:

```bash
sudo nano /etc/redis/redis.conf
```

**Key settings**:

```conf
# Bind to all interfaces (for network access)
bind 0.0.0.0

# Set password (IMPORTANT for security)
requirepass your_secure_password_here

# Performance settings
maxmemory 256mb
maxmemory-policy allkeys-lru

# Persistence (optional but recommended)
save 900 1
save 300 10
save 60 10000
```

**Restart Redis**:

```bash
sudo systemctl restart redis-server
```

#### Step 2: Configure Each Minecraft Server

On **each** server in your network:

```yaml
# config.yml
redis:
  # Enable Redis
  enabled: true
  
  # Unique identifier for THIS server
  server-id: "lobby-1"  # Change for each server!
  
  # Redis connection
  host: "localhost"      # Redis server IP
  port: 6379            # Redis port
  password: "your_secure_password_here"
  database: 0           # Redis database number (0-15)
  timeout: 2000         # Connection timeout (ms)
  use-ssl: false        # Enable SSL if needed
  
  # Connection pool (advanced)
  pool:
    max-total: 8        # Maximum connections
    max-idle: 8         # Maximum idle connections
    min-idle: 0         # Minimum idle connections
```

#### Step 3: Set Unique Server IDs

**CRITICAL**: Each server must have a unique `server-id`:

```yaml
# Lobby server
redis:
  server-id: "lobby-1"

# Survival server
redis:
  server-id: "survival-1"

# Creative server
redis:
  server-id: "creative-1"

# Minigames server
redis:
  server-id: "minigames-1"
```

#### Step 4: Use MySQL (Recommended)

For cross-server, use MySQL instead of SQLite:

```yaml
# config.yml
database:
  type: "mysql"
  
  mysql:
    host: "mysql.example.com"
    port: 3306
    database: "blackmarket"
    username: "blackmarket"
    password: "password"
```

All servers should connect to the **same MySQL database**.

#### Step 5: Restart All Servers

After configuration, restart all servers to connect to Redis.

***

### Verifying Redis Connection

#### Check Server Logs

Look for these messages on startup:

```
[BlackMarket] Redis is enabled - initializing cross-server support...
[BlackMarket] Redis connection established successfully!
[BlackMarket]   Server ID: lobby-1
[BlackMarket]   Cross-server synchronization enabled
```

#### Test In-Game

1. **Buy an item** on Server 1
2. **Check stock** on Server 2
3. Stock should be updated instantly

#### Test Commands

```
# On any server
/bma info

# Should show:
# Redis: ENABLED (Cross-Server Mode)
```

***

### How It Works

#### Market Synchronization

When market resets on any server:

```
Server 1: /bma reset
    ↓
Redis: Broadcasts "market_reset" message
    ↓
Server 1, 2, 3, 4: All receive and update their markets
    ↓
All players see the same items
```

#### Purchase Synchronization

When a player buys an item:

```
Player buys on Server 1
    ↓
Server 1: Deducts stock, updates database
    ↓
Redis: Broadcasts "purchase" message
    ↓
Server 2, 3, 4: Update their cached stock
    ↓
All servers show correct remaining stock
```

#### Data Flow

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Server 1   │────▶│    Redis     │◀────│   Server 2   │
│   (Lobby)    │     │   (Pub/Sub)  │     │  (Survival)  │
└──────────────┘     └──────────────┘     └──────────────┘
        │                     │                     │
        └─────────────────────┴─────────────────────┘
                              │
                        ┌─────▼─────┐
                        │   MySQL   │
                        │ (Storage) │
                        └───────────┘
```

***

### Advanced Configuration

#### Remote Redis Server

If Redis is on a different machine:

```yaml
redis:
  enabled: true
  server-id: "lobby-1"
  host: "192.168.1.100"  # Remote Redis IP
  port: 6379
  password: "password"
```

**Firewall**: Ensure port 6379 is open:

```bash
# UFW
sudo ufw allow 6379/tcp

# Firewalld
sudo firewall-cmd --permanent --add-port=6379/tcp
sudo firewall-cmd --reload
```

#### SSL/TLS Connection

For secure Redis connections:

```yaml
redis:
  enabled: true
  host: "redis.example.com"
  port: 6380  # Custom SSL port
  use-ssl: true
  password: "password"
```

**Redis SSL setup** (requires stunnel or Redis 6+ with TLS):

```conf
# redis.conf
tls-port 6380
tls-cert-file /etc/redis/redis.crt
tls-key-file /etc/redis/redis.key
tls-ca-cert-file /etc/redis/ca.crt
```

#### Multiple Redis Databases

Separate different server groups:

```yaml
# Production servers
redis:
  database: 0  # Production

# Test servers
redis:
  database: 1  # Testing
```

#### Connection Pool Tuning

For high-traffic servers:

```yaml
redis:
  pool:
    max-total: 16      # More connections
    max-idle: 8
    min-idle: 2        # Keep connections ready
```

***

### Redis Data Structure

#### Cached Data

BlackMarket stores these in Redis:

| Key                        | Type   | Purpose              | TTL      |
| -------------------------- | ------ | -------------------- | -------- |
| `blackmarket:global_items` | List   | Current market items | 1 hour   |
| `blackmarket:last_reset`   | String | Last reset timestamp | 24 hours |
| `blackmarket:stock:<item>` | String | Item stock count     | 1 hour   |

#### Pub/Sub Channels

| Channel                     | Purpose                   |
| --------------------------- | ------------------------- |
| `blackmarket:market_update` | Market items changed      |
| `blackmarket:purchase`      | Item purchased            |
| `blackmarket:reset`         | Market reset              |
| `blackmarket:sync_request`  | Server requests sync      |
| `blackmarket:sync_response` | Server provides sync data |

***

### Monitoring Redis

#### Check Connected Servers

```bash
redis-cli -h localhost -p 6379 -a password
```

```redis
# List all BlackMarket keys
KEYS blackmarket:*

# Check server connections
CLIENT LIST

# Monitor real-time activity
MONITOR
```

#### View Cached Data

```redis
# Get global market items
LRANGE blackmarket:global_items 0 -1

# Get last reset time
GET blackmarket:last_reset

# Get stock for item
GET blackmarket:stock:diamond_sword
```

#### Performance Monitoring

```redis
# Server stats
INFO stats

# Memory usage
INFO memory

# Connection count
INFO clients
```

***

### Troubleshooting

#### Issue 1: Redis Connection Failed

**Error**: `Failed to initialize Redis`

**Solutions**:

1. **Check Redis is running**:

```bash
sudo systemctl status redis-server
```

2. **Test connection**:

```bash
redis-cli -h localhost -p 6379 -a password ping
# Should return: PONG
```

3. **Check firewall**:

```bash
telnet redis-host 6379
```

4. **Verify password**:

```yaml
redis:
  password: "correct_password"
```

#### Issue 2: Data Not Syncing

**Problem**: Changes on one server don't appear on others

**Solutions**:

1. **Check server IDs are unique**:

```yaml
# Each server must have different ID
server-id: "lobby-1"   # NOT the same on all!
```

2. **Verify all servers connected**:

```
/bma info
# Should show: Redis: ENABLED
```

3. **Check Redis channels**:

```redis
# In redis-cli
PUBSUB CHANNELS blackmarket:*
```

4. **Enable debug mode**:

```
/bma debug
# Check logs for Redis messages
```

#### Issue 3: High Latency

**Problem**: Slow synchronization between servers

**Solutions**:

1. **Check network latency**:

```bash
ping redis-host
```

2. **Increase timeout**:

```yaml
redis:
  timeout: 5000  # 5 seconds
```

3. **Use local Redis** (on each server):

```yaml
redis:
  host: "localhost"
```

4. **Optimize Redis**:

```conf
# redis.conf
tcp-keepalive 300
timeout 0
```

#### Issue 4: Duplicate Server IDs

**Error**: Servers showing same data incorrectly

**Solution**: Ensure unique IDs on each server:

```bash
# Check each server's config
grep "server-id" config.yml
```

***

### Best Practices

#### 1. Use MySQL with Redis

✅ **Do**:

```yaml
database:
  type: "mysql"  # Shared database

redis:
  enabled: true  # Synchronized cache
```

❌ **Don't**:

```yaml
database:
  type: "sqlite"  # Each server has own file

redis:
  enabled: true   # Won't work properly!
```

#### 2. Secure Your Redis

✅ **Do**:

```conf
# redis.conf
requirepass strong_password_here
bind 127.0.0.1  # Or specific IPs only
```

❌ **Don't**:

```conf
bind 0.0.0.0     # Exposed to internet
# No password      # Anyone can connect
```

#### 3. Monitor Redis Memory

```conf
# redis.conf
maxmemory 256mb
maxmemory-policy allkeys-lru
```

Check usage:

```redis
INFO memory
```

#### 4. Enable Persistence

```conf
# redis.conf
save 900 1      # Save after 900s if 1 key changed
save 300 10     # Save after 300s if 10 keys changed
save 60 10000   # Save after 60s if 10000 keys changed
```

***

### Network Topologies

#### Topology 1: Single Redis (Simple)

```
┌─────────┐
│  Redis  │ ← All servers connect here
└────┬────┘
     │
     ├──────┬──────┬──────┐
     │      │      │      │
  Server Server Server Server
    1      2      3      4
```

**Pros**: Simple setup **Cons**: Single point of failure

#### Topology 2: Redis per Server (Advanced)

```
Server 1 ──▶ Redis 1 ─┐
Server 2 ──▶ Redis 2 ─┼─ Redis Cluster
Server 3 ──▶ Redis 3 ─┤  (synchronized)
Server 4 ──▶ Redis 4 ─┘
```

**Pros**: Better performance, no single point of failure **Cons**: Complex setup (requires Redis Cluster)

#### Topology 3: Central + Cache

```
     ┌──────────────┐
     │ Central Redis│ ← Master
     └───────┬──────┘
             │
    ┌────────┴────────┐
    │                 │
Local Redis      Local Redis  ← Caches
    │                 │
Server 1         Server 2
```

**Pros**: Fast local reads, synchronized writes **Cons**: Most complex setup

***

### Performance Tips

#### 1. Use Connection Pooling

Already enabled by default in BlackMarket via HikariCP.

#### 2. Minimize Network Hops

Place Redis close to Minecraft servers:

* Same datacenter
* Low latency network
* Consider local Redis per server

#### 3. Use Pipeline for Bulk Operations

BlackMarket automatically batches operations for efficiency.

#### 4. Monitor and Optimize

```bash
# Check slow operations
redis-cli --latency

# Monitor commands
redis-cli monitor

# Check memory fragmentation
redis-cli info memory | grep fragmentation
```

***

### Migration Guide

#### Moving from Standalone to Cross-Server

**Step 1**: Set up Redis on all servers

**Step 2**: Migrate to MySQL (if using SQLite):

```
/bma migrate mysql confirm
```

**Step 3**: Enable Redis on each server:

```yaml
redis:
  enabled: true
  server-id: "unique-id"
```

**Step 4**: Sync data across servers:

* First server to start will be the "master"
* Other servers will sync from Redis cache

**Step 5**: Test synchronization:

```
# Server 1
/bma reset

# Server 2
/blackmarket
# Should show same items
```

***

### Next Steps

* [Set up MySQL database](database.md#mysql-configuration)
* [Configure economy integration](economy-integration.md)
* [Learn about troubleshooting](troubleshooting-guide.md)
* [View FAQ](frequently-asked-questions.md)

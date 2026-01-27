# 🔑 Commands & Permissions

Complete reference for all BlackMarket commands and permissions.

### Player Commands

#### `/blackmarket`

Opens the BlackMarket GUI.

**Aliases**: `/bm`, `/market`

**Permission**: `blackmarket.use`

**Usage**:

```
/blackmarket
/blackmarket help
/blackmarket info
```

**Subcommands**:

| Subcommand | Description              | Permission        |
| ---------- | ------------------------ | ----------------- |
| _(none)_   | Opens market GUI         | `blackmarket.use` |
| `help`     | Shows help message       | `blackmarket.use` |
| `info`     | Shows market information | `blackmarket.use` |

***

### Admin Commands

#### `/blackmarketadmin`

Administrative commands for managing the market.

**Aliases**: `/bma`, `/bmadmin`

**Permission**: `blackmarket.admin`

**Usage**: `/bma <subcommand> [args]`

#### Subcommands Reference

**`/bma help`**

Displays all admin commands.

```
/bma help
```

**Permission**: `blackmarket.admin`

***

**`/bma reload`**

Reloads configuration files.

```
/bma reload [config|messages|items|all]
```

**Permission**: `blackmarket.admin.reload`

**Arguments**:

* `config` - Reload config.yml only
* `messages` - Reload messages/language files only
* `items` - Reload items.yml only
* `all` - Reload everything (default)

**Examples**:

```
/bma reload              # Reloads everything
/bma reload config       # Reloads config.yml
/bma reload messages     # Reloads message files
/bma reload items        # Reloads items.yml
```

***

**`/bma reset`**

Forces an immediate market reset.

```
/bma reset
```

**Alias**: `/bma refresh`

**Permission**: `blackmarket.admin.reset`

**Effects**:

* Rotates market items immediately
* Resets all purchase data
* Clears cooldowns and stock counts
* Updates next reset time

**Warning**: This affects all players and cannot be undone!

***

**`/bma open`**

Opens the market GUI for another player.

```
/bma open <player>
```

**Permission**: `blackmarket.admin.open`

**Arguments**:

* `<player>` - Target player name

**Examples**:

```
/bma open Steve
/bma open Notch
```

**Use Cases**:

* Testing as another player
* Customer support
* Demonstrating features

***

**`/bma info`**

Shows detailed plugin information.

```
/bma info
```

**Permission**: `blackmarket.admin.info`

**Displays**:

* Plugin version
* Server type (Bukkit/Spigot/Paper/Folia)
* Market type (Global/Per-Player)
* Database type (SQLite/MySQL)
* Economy provider
* Online players
* Current language
* Active market items (if global)

***

**`/bma debug`**

Toggles debug mode on/off.

```
/bma debug
```

**Permission**: `blackmarket.admin.debug`

**Effects**:

* Enables/disables verbose logging
* Shows detailed system information
* Helps diagnose issues

**Debug Information Shown**:

* Folia support status
* Reset interval
* Items per reset
* Pagination status
* Categories status
* MiniMessage status
* Localization status
* Current language

***

**`/bma migrate`**

Migrates data between database types.

```
/bma migrate <sqlite|mysql> [confirm]
```

**Permission**: `blackmarket.admin.migrate`

**Arguments**:

* `sqlite` - Migrate to SQLite
* `mysql` - Migrate to MySQL
* `confirm` - Confirmation flag (required)

**Process**:

1. Run without confirm to see warning
2. Run with confirm to execute migration
3. Wait for completion message
4. Update config.yml database type
5. Reload plugin

**Examples**:

```
# Step 1: View migration info
/bma migrate mysql

# Step 2: Confirm and execute
/bma migrate mysql confirm

# Step 3: Update config.yml
database:
  type: "mysql"  # Change from "sqlite"

# Step 4: Reload
/bma reload
```

**What Gets Migrated**:

* ✅ Player data
* ✅ Purchase history
* ✅ Player market items
* ✅ Global market items
* ✅ Item sold counts
* ✅ Purchase records
* ✅ Global settings

**Warning**: Always backup your database before migrating!

***

### Complete Permissions List

#### Player Permissions

| Permission               | Description                   | Default |
| ------------------------ | ----------------------------- | ------- |
| `blackmarket.use`        | Access `/blackmarket` command | `true`  |
| `blackmarket.buy.<item>` | Purchase specific item        | `true`  |

#### Admin Permissions

| Permission                  | Description               | Default |
| --------------------------- | ------------------------- | ------- |
| `blackmarket.admin`         | Access all admin commands | `op`    |
| `blackmarket.admin.reload`  | Reload configuration      | `op`    |
| `blackmarket.admin.reset`   | Force market reset        | `op`    |
| `blackmarket.admin.open`    | Open market for others    | `op`    |
| `blackmarket.admin.info`    | View plugin info          | `op`    |
| `blackmarket.admin.debug`   | Toggle debug mode         | `op`    |
| `blackmarket.admin.migrate` | Migrate databases         | `op`    |

#### Item-Specific Permissions

You can require permissions for individual items using the `permission` property:

```yaml
# items.yml
legendary_sword:
  material: "NETHERITE_SWORD"
  display-name: "&4Legendary Sword"
  price: 1000000
  permission: "blackmarket.buy.legendary"  # Requires this permission
```

**Common Permission Patterns**:

```
blackmarket.buy.weapons
blackmarket.buy.ranks
blackmarket.buy.legendary
blackmarket.buy.vip
blackmarket.buy.premium
```

***

### Permission Examples

#### LuckPerms Setup

```yaml
# Basic player access
/lp group default permission set blackmarket.use true

# VIP players can buy special items
/lp group vip permission set blackmarket.buy.vip true

# Admin full access
/lp group admin permission set blackmarket.admin true

# Moderator can reload
/lp group mod permission set blackmarket.admin.reload true
```

#### GroupManager Setup

```yaml
groups:
  default:
    permissions:
      - blackmarket.use
  
  vip:
    permissions:
      - blackmarket.buy.vip
      - blackmarket.buy.premium
  
  admin:
    permissions:
      - blackmarket.admin
```

#### PermissionsEx Setup

```yaml
groups:
  default:
    - blackmarket.use
  
  moderator:
    - blackmarket.admin.reload
    - blackmarket.admin.info
  
  admin:
    - blackmarket.admin
```

***

### Permission Hierarchies

#### Recommended Setup

```
blackmarket.admin (parent)
├── blackmarket.admin.reload
├── blackmarket.admin.reset
├── blackmarket.admin.open
├── blackmarket.admin.info
├── blackmarket.admin.debug
└── blackmarket.admin.migrate

blackmarket.buy.* (parent for all items)
├── blackmarket.buy.weapons
├── blackmarket.buy.ranks
├── blackmarket.buy.legendary
└── blackmarket.buy.vip
```

#### Wildcard Permissions

Grant all permissions at once:

```yaml
# All player permissions
blackmarket.use

# All admin permissions
blackmarket.admin.*

# All buying permissions
blackmarket.buy.*

# Everything
blackmarket.*
```

***

### Command Blocks

All commands can be executed from command blocks:

```
/execute as @p run blackmarket
/bma reset
/bma open @p
```

**Note**: Command blocks always have operator permissions.

***

### Console Commands

All admin commands work from console:

```
bma reload
bma reset
bma info
bma debug
```

**Player-specific commands require player argument**:

```
bma open Steve
```

***

### Tab Completion

#### `/blackmarket`

```
/blackmarket <TAB>
  help
  info
```

#### `/bma`

```
/bma <TAB>
  help
  reload
  reset
  refresh
  open
  info
  debug
  migrate

/bma reload <TAB>
  all
  config
  messages
  items

/bma open <TAB>
  <online_player_names>

/bma migrate <TAB>
  sqlite
  mysql

/bma migrate mysql <TAB>
  confirm
```

***

### Command Cooldowns

You can add cooldowns to commands using external plugins like DeluxeCommands or CommandCooldown:

```yaml
# Example with DeluxeCommands
cooldowns:
  blackmarket:
    time: 60  # 60 seconds between uses
```

***

### Command Aliases

#### Creating Custom Aliases

**Using Bukkit commands.yml**:

```yaml
# commands.yml
aliases:
  shop: blackmarket
  market: blackmarket
  bm: blackmarket
  store: blackmarket
  bmr: bma reload
```

**Using EssentialsX**:

```yaml
# config.yml
custom-commands:
  shop:
    - blackmarket
```

***

### Common Use Cases

#### Daily Market Check

```bash
# Schedule with cron/task scheduler
/bma info  # Check current items
```

#### Automated Resets

```bash
# Use plugin like CommandTimer or AutoCommander
# Every 24 hours:
/bma reset
```

#### Testing Items

```bash
# Reload items after editing
/bma reload items

# Test as another player
/bma open TestPlayer

# Force new rotation
/bma reset
```

#### Debugging Issues

```bash
# Enable debug mode
/bma debug

# Check plugin info
/bma info

# Check logs
# Look in console for detailed output
```

***

### Next Steps

* [Learn about placeholders](placeholders.md)
* [Configure items](items-configuration.md)
* [Set up the GUI](gui-customization.md)
* [Troubleshooting guide](troubleshooting-guide.md)

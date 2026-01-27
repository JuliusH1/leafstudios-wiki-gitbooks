# 📥 Installation & Setup

## 📥 Installation & Setup

### Requirements

#### Required

* **Minecraft Server**: 1.16.5 - 1.21+
* **Server Software**: Spigot, Paper, Purpur, or Folia
* **Economy Plugin**: One of the following:
  * Vault (with compatible economy plugin)
  * PlayerPoints
  * CoinsEngine
  * Built-in Experience Levels (no plugin needed)

#### Optional

* **PlaceholderAPI**: For placeholder support in other plugins
* **Redis**: For cross-server synchronization on BungeeCord/Velocity networks

### Installation Steps

#### 1. Download the Plugin

Download the latest version of BlackMarket from [BuiltByBit](https://builtbybit.com/resources/blackmarket-endless-customization.73633/).

#### 2. Install Dependencies

Install at least one economy plugin:

**Vault Setup:**

```
1. Download Vault from SpigotMC
2. Download an economy plugin (EssentialsX, CMI, etc.)
3. Place both in your plugins folder
4. Restart the server
```

**PlayerPoints Setup:**

```
1. Download PlayerPoints
2. Place in plugins folder
3. Restart the server
```

**CoinsEngine Setup:**

```
1. Download CoinsEngine
2. Place in plugins folder
3. Configure currencies
4. Restart the server
```

#### 3. Install BlackMarket

```bash
# Stop your server
stop

# Place BlackMarket.jar in your plugins folder
plugins/BlackMarket.jar

# Start your server
start
```

#### 4. Initial Configuration

On first startup, BlackMarket will generate three configuration files:

```
plugins/BlackMarket/
├── config.yml          # Main configuration
├── items.yml           # Item definitions
├── messages.yml        # Default messages (if localization disabled)
└── lang/               # Language files (if localization enabled)
    └── EN_US.yml
```

#### 5. Configure Your Economy

Open `config.yml` and set your default economy:

```yaml
economy:
  default: "vault"  # Options: vault, playerpoints, coinsengine, exp
```

#### 6. Test the Installation

Run these commands to verify everything works:

```
/blackmarket          # Opens the market GUI
/bma info            # Shows plugin information
/bma reload          # Reloads configuration
```

### Post-Installation

#### Recommended Setup

1. **Configure Market Items** - Edit `items.yml` to add your custom items
2. **Customize GUI** - Modify `config.yml` GUI section for your preferred layout
3. **Set Permissions** - Configure player and admin permissions
4. **Test Economy Integration** - Verify purchases work with your economy plugin

#### Optional Features

**Enable PlaceholderAPI Support**

```
1. Install PlaceholderAPI
2. Restart server
3. BlackMarket placeholders will automatically register
```

**Enable Redis (Cross-Server)**

```yaml
redis:
  enabled: true
  server-id: "lobby-1"  # Unique ID per server
  host: "localhost"
  port: 6379
```

See [Redis & Cross-Server](redis-and-cross-server-synchronization.md) for detailed setup.

**Enable Localization**

```yaml
messages:
  localization: true
  lang: "EN_US"  # Place custom language files in lang/
```

### Next Steps

* [Configure your market items](items-configuration.md)
* [Customize the GUI](gui-customization.md)
* [Set up permissions](commands-and-permissions.md)
* [Learn about placeholders](placeholders.md)

### Troubleshooting

If you encounter issues during installation:

1. **Check server logs** - Look for errors in `logs/latest.log`
2. **Verify dependencies** - Ensure economy plugin is loaded
3. **Check file permissions** - Plugin folder must be writable
4. **Review configuration** - Validate YAML syntax (use YAML linter)

For more help, see [Troubleshooting](troubleshooting-guide.md).

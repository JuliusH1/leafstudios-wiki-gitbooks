# 📥 Installation & Setup

### 📋 Requirements

* **Server Software**: Paper 1.21+ or Folia 1.21+
* **Java Version**: Java 17 or higher
* **Dependencies**: None required (PlaceholderAPI and LuckPerms are optional)

### 📥 Installation Steps

#### 1. Download the Plugin

1. Download the latest `BlockyJoin.jar` file from:
   * [BuiltByBit](https://builtbybit.com/resources/blockyjoin.91828/)

#### 2. Install on Your Server

1. **Stop your server** if it's currently running
2. Place `BlockyJoin.jar` in your server's `plugins` folder
3. **Start your server**

#### 3. Verify Installation

Check your server console for the BlockyJoin startup message:

```
[BlockyJoin] -------------------------------------------------------------
[BlockyJoin]  ____   _               _                _         _        
[BlockyJoin] | __ ) | |  ___    ___ | | __ _   _     | |  ___  (_) _ __  
[BlockyJoin] |  _ \ | | / _ \  / __|| |/ /| | | | _  | | / _ \ | || '_ \ 
[BlockyJoin] | |_) || || (_) || (__ |   < | |_| || |_| || (_) || || | | |
[BlockyJoin] |____/ |_| \___/  \___||_|\_\ \__, | \___/  \___/ |_||_| |_|
[BlockyJoin]                              |___/                         
[BlockyJoin] -------------------------------------------------------------
[BlockyJoin] BlockyJoin v1.0.0 by LeafStudios x Blockyware enabled!
```

#### 4. Configure the Plugin

After first startup, BlockyJoin will generate default configuration files in `plugins/BlockyJoin/`:

```
plugins/BlockyJoin/
├── config.yml                    # Main configuration
├── messages.yml                  # Plugin messages
├── menu.yml                      # GUI configuration
├── join-leave-messages.yml       # Join/leave message sets
└── blockyjoin.db            # SQLite database
```

### 🔌 Optional Dependencies

#### PlaceholderAPI (Recommended)

PlaceholderAPI allows you to use thousands of additional placeholders in your messages.

1. Download from [SpigotMC](https://www.spigotmc.org/resources/placeholderapi.6245/)
2. Place in `plugins` folder
3. Restart your server

**Example usage:**

```yaml
join:
  - "<#49FF00>[+] %luckperms_prefix% {player} %player_ping%ms"
```

#### LuckPerms (Optional)

LuckPerms integration provides built-in prefix, suffix, and group placeholders.

1. Download from [LuckPerms.net](https://luckperms.net/)
2. Place in `plugins` folder
3. Restart your server

**Built-in placeholders:**

* `{player_prefix}` - Player's LuckPerms prefix
* `{player_suffix}` - Player's LuckPerms suffix
* `{player_group}` - Player's primary group

### 🎮 First-Time Setup

#### 1. Set Permissions

Grant players access to the GUI:

Use /lp user \<player> permission set blockyjoin.gui to grant users permission to open the GUI

#### 2. Test the Plugin

As a player, run:

```
/blockyjoin
```

This should open the message selection GUI.

#### 3. Customize Message Sets

Edit `join-leave-messages.yml` to add your own message sets or modify existing ones. See [Creating Message Sets](creating-message-sets.md) for details.

#### 4. Configure Settings

Edit `config.yml` to customize:

* Default message set
* First join messages
* GUI click actions
* World restrictions

### 🔄 Updating

#### Automatic Config Updates

BlockyJoin includes an automatic config updater that:

* ✅ Preserves all your custom settings
* ✅ Adds new default values for new features
* ✅ Creates backups before updating
* ✅ Shows warnings for deprecated options

#### Update Process

1. **Backup your server** (recommended but not required)
2. **Stop your server**
3. **Replace** the old jar file with the new one
4. **Start your server**
5. **Check console** for update messages
6. **Review** any warnings about config changes

### 🐛 Troubleshooting

#### Plugin Not Loading

**Problem:** Plugin doesn't appear in `/plugins` list

**Solutions:**

1. Verify Java 17+ is installed: `/version`
2. Check for errors in `logs/latest.log`
3. Ensure you're using Paper/Folia 1.21+
4. Try deleting and re-downloading the jar

#### Database Errors

**Problem:** Database-related errors in console

**Solutions:**

1. Check file permissions on `plugins/BlockyJoin/`
2. Delete `blockyjoin.db` to regenerate (will lose player selections)

#### Messages Not Showing

**Problem:** Join/leave messages don't appear

**Solutions:**

1. Check `config.yml` - ensure `enabled: true`
2. Verify world is not in `disabled-worlds` list
3. Run `/blockyjoin reload` after config changes
4. Check for conflicting plugins

#### GUI Not Opening

**Problem:** `/blockyjoin` doesn't open GUI

**Solutions:**

1. Verify permission: `blockyjoin.gui`
2. Check for console errors
3. Ensure at least one message set exists in `join-leave-messages.yml`

### ✅ Next Steps

Now that BlockyJoin is installed, you can:

1. [Learn the Commands](commands-and-permissions.md) - Master all available commands
2. [Create Custom Message Sets](creating-message-sets.md) - Design unique messages
3. [Configure the GUI](gui-customization.md) - Personalize the interface

***

Need help? Join our [Discord](https://leafstudios.dev/discord) or check the [FAQ](frequently-asked-questions.md).

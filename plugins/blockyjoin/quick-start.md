---
description: Get BlockyJoin up and running in minutes!
---

# 🔌 Quick Start

### 📋 Prerequisites

Before starting, ensure you have:

* ✅ Paper 1.21+ or Folia 1.21+ server
* ✅ Java 17 or higher
* ✅ Basic knowledge of Minecraft server administration

### 🚀 5-Minute Setup

#### Step 1: Install the Plugin (1 minute)

1. **Download** BlockyJoin jar file
2. **Place** it in your `plugins` folder
3. **Restart** your server

```bash
# Expected console output:
[BlockyJoin] BlockyJoin v1.0.0 by LeafStudios x Blockyware enabled!
[BlockyJoin] Loaded 35 join/leave message sets
```

#### Step 2: Grant Basic Permissions (1 minute)

Give all players access to the GUI and default message set:

```bash
/lp group default permission set blockyjoin.gui true
/lp group default permission set blockyjoin.set.default true
```

**Using PermissionsEx or GroupManager?** See [Commands & Permissions](commands-and-permissions.md).

#### Step 3: Test the Plugin (1 minute)

As a player, run:

```
/blockyjoin
```

You should see the message selection GUI open with various message sets!

#### Step 4: Select a Message Set (1 minute)

1. **Click** on any message set in the GUI
2. **Confirm** your selection (if confirmation GUI is enabled)
3. **Test** your messages with `/blockyjoin test join`

#### Step 5: Customize (Optional, 1 minute)

Edit `plugins/BlockyJoin/config.yml` to customize:

```yaml
first-join:
  enabled: true
  messages:
    - "<#CB1FE9>Welcome {player} to MyServer!"
```

Then reload:

```
/blockyjoin reload
```

### ✨ That's It!

Your join/leave messages are now active! Players can:

* Open GUI with `/blockyjoin`
* Select their preferred message set
* See custom join/leave messages

### 🎯 Next Steps

#### Basic Customization

**Add a VIP message set:**

```bash
# Grant VIP rank access to special sets
/lp group vip permission set blockyjoin.set.king true
/lp group vip permission set blockyjoin.set.wizard true
```

**Change default message set:**

```yaml
# In config.yml
default-message-set: "default"
```

**Customize GUI title:**

```yaml
# In menu.yml
main-gui:
  title: "Select Your Join Message"
```

#### Advanced Features

**Enable PlaceholderAPI support:**

1. Download [PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/)
2. Place in plugins folder
3. Restart server
4. Use PAPI placeholders in messages: `%player_ping%`

**Create a custom message set:**

```yaml
# In join-leave-messages.yml
messages:
  custom:
    join:
      - "<#49FF00>[+] <gradient:#FF0000:#0000FF>{player}</gradient> joined!"
    leave:
      - "<#FF3232>[-] {player} left!"
    item:
      material: "DIAMOND"
      name: "<#00FFFF>Custom Set"
      lore:
        - "My custom message set"
    permission: "blockyjoin.set.custom"
```

Then reload: `/blockyjoin reload`

### 🎮 Player Tutorial

Share this with your players:

**How to Change Your Join Message:**

1. Type `/blockyjoin` in chat
2. A menu will open showing available message sets
3. Click on the message set you want
4. Confirm your selection
5. Done! Your new messages are active

**How to Favorite Message Sets:**

1. Open `/blockyjoin`
2. **Shift+Left-Click** on a message set to favorite it
3. Click the **Favorites** button to see only your favorites
4. **Shift+Right-Click** to remove from favorites

### 📝 Common First Tasks

#### 1. Set Up Rank-Based Message Sets

```bash
# Default players get basic sets
/lp group default permission set blockyjoin.set.default true

# VIP gets special sets
/lp group vip permission set blockyjoin.set.king true
/lp group vip permission set blockyjoin.set.wizard true
/lp group vip permission set blockyjoin.set.superhero true

# VIP+ gets even more
/lp group vipplus permission set blockyjoin.set.dragon true
/lp group vipplus permission set blockyjoin.set.phoenix true
```

#### 2. Configure First Join Messages

```yaml
# In config.yml
first-join:
  enabled: true
  random-message: true
  messages:
    - "<#CB1FE9>⛏ Welcome {player} to MyServer!"
    - "<#49FF00>Everyone welcome {player} to the server!"
    - "<gradient:#FF0000:#00FF00>{player} just joined for the first time!</gradient>"
```

#### 3. Disable Messages in Certain Worlds

```yaml
# In config.yml
disabled-worlds:
  - world_nether
  - world_the_end
  - creative_world
```

#### 4. Customize GUI Appearance

```yaml
# In menu.yml
main-gui:
  title: "   <st>     <reset> <dark_gray>MY SERVER <st>     "
  rows: 6
  close-on-select: false
```

#### 5. Add Custom Sounds

```yaml
# In menu.yml
sounds:
  select:
    enabled: true
    sound: "entity.player.levelup"
    volume: 1.0
    pitch: 1.2
```

### 🎨 Quick Customization Examples

#### Example 1: Simple Server Branding

```yaml
# In config.yml - Replace default messages
join:
  messages:
    - "<#49FF00>[+] {player} <gray>| <white>Welcome to MyServer!"

leave:
  messages:
    - "<#FF3232>[-] {player} <gray>| <white>See you soon!"

first-join:
  messages:
    - "<#CB1FE9>⛏ Everyone welcome {player} to MyServer! <gray>[#{join_count}]"
```

#### Example 2: Stats-Based Messages (with PAPI)

```yaml
join:
  messages:
    - "<#49FF00>[+] {player} <gray>| Ping: %player_ping%ms | Balance: $%vault_eco_balance%"
```

#### Example 3: LuckPerms Ranks

```yaml
join:
  messages:
    - "<#49FF00>[+] {luckperms_prefix} {player} {luckperms_suffix}"
```

### ⚡ Performance Tips

1. **Use SQLite** (default) - Already optimized for most servers
2. **Limit message variants** - 2-5 per set is ideal
3. **Optimize placeholders** - Too many PAPI placeholders may slow down joins
4. **Regular backups** - BlockyJoin does this automatically

### 🔍 Verification Checklist

After setup, verify everything works:

* Plugin loads without errors
* `/blockyjoin` opens the GUI
* Players can select message sets
* Join messages display correctly
* Leave messages display correctly
* Permissions work as expected
* First join messages work (if enabled)
* Config changes apply after `/blockyjoin reload`

### 🆘 Quick Troubleshooting

**GUI won't open?**

* Check permission: `blockyjoin.gui`
* Verify at least one message set exists
* Check console for errors

**Messages not showing?**

* Verify `enabled: true` in config
* Check world isn't in `disabled-worlds`
* Disable conflicting plugins

**Placeholders not working?**

* Internal: Use `{player}`
* PAPI: Use `%player_name%` and install PlaceholderAPI
* LuckPerms: Use `{luckperms_prefix}` and install LuckPerms

**Colors showing as text?**

* Use MiniMessage: `<#FF0000>` not `§c`
* Format: `<#RRGGBB>Red Text`

### 📚 Learn More

* [Full Installation Guide](installation-and-setup.md) - Detailed installation instructions
* [Commands & Permissions](installation-and-setup.md) - Complete command reference
* [Creating Message Sets](creating-message-sets.md) - Make your own message sets
* [Placeholders](placeholders.md) - All available placeholders
* [FAQ](frequently-asked-questions.md) - Common questions and answers

### 💬 Need Help?

* **Discord**: [Join our Discord](https://leafstudios.dev/discord)
* **Wiki**: [Full Documentation](../blockyjoin.md)
* **Support**: support@leafstudios.dev

***

### 🎉 You're All Set!

BlockyJoin is now ready to use. Your players can enjoy customizable join/leave messages, and you can expand on this setup anytime by:

* Adding more message sets
* Creating custom themes
* Integrating with PlaceholderAPI
* Setting up rank-based access
* Customizing the GUI

**Have fun customizing your server's join/leave messages!** 🚀

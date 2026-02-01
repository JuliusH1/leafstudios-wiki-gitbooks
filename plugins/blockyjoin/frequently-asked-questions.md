---
description: Common questions and answers about BlockyJoin.
---

# ❓ Frequently Asked Questions

### 🚀 General Questions

#### What is BlockyJoin?

BlockyJoin is a premium join/leave message plugin that allows players to choose from multiple message sets through a beautiful GUI. It supports custom messages, PlaceholderAPI, LuckPerms integration, and runs on both Paper and Folia servers.

#### What Minecraft versions are supported?

BlockyJoin supports:

* Paper 1.21+
* Folia 1.21+
* Requires Java 17 or higher

#### Does BlockyJoin work on Spigot?

BlockyJoin is designed for **Paper** and **Folia** servers. While it may work on Spigot, we officially support and test only on Paper/Folia. For best results, use Paper 1.20+.

#### Is BlockyJoin compatible with Folia?

Yes! BlockyJoin is fully compatible with Folia and uses regionized scheduling for optimal performance on Folia's multi-threaded architecture.

### 🔌 Plugin Compatibility

#### Does this work with PlaceholderAPI?

Yes! BlockyJoin fully supports PlaceholderAPI. Simply install PlaceholderAPI and you can use any PAPI placeholder in your messages using `%placeholder%` syntax.

**Example:**

```yaml
join:
  - "<#49FF00>[+] {player} <gray>| Ping: %player_ping%ms | Balance: $%vault_eco_balance%"
```

#### Does this work with LuckPerms?

Yes! BlockyJoin automatically detects LuckPerms and provides built-in placeholders:

* `{player_prefix}` - Player's prefix
* `{player_suffix}` - Player's suffix
* `{player_group}` - Player's primary group

#### Will BlockyJoin conflict with other join message plugins?

BlockyJoin cancels the default Minecraft join/leave messages. If you have other join message plugins installed, disable them to avoid conflicts.

**Common conflicts:**

* EssentialsX join messages (disable in Essentials config)
* Other join message plugins
* MOTD plugins that modify join messages

#### Can I use this with Vault?

Yes, through PlaceholderAPI! Install PlaceholderAPI and the Vault expansion to use economy and permission placeholders.

### ⚙️ Configuration

#### How do I add a custom message set?

1. Open `plugins/BlockyJoin/join-leave-messages.yml`
2. Add your message set following this structure:

```yaml
messages:
  your-id:
    join:
      - "Your join message"
    leave:
      - "Your leave message"
    item:
      material: "DIAMOND"
      name: "<gold>Your Set Name"
      lore:
        - "Description"
    permission: "blockyjoin.set.your-id"
```

3. Run `/blockyjoin reload`
4. Grant permission: `/lp group <group> permission set blockyjoin.set.your-id true`

See [Creating Message Sets](creating-message-sets.md) for detailed instructions.

#### How do I add custom player heads to GUI items?

1. Visit [Minecraft-Heads.com](https://minecraft-heads.com/)
2. Find a head you like
3. Click "Get Value" and copy the base64 string
4. Add to your message set:

```yaml
item:
  material: "PLAYER_HEAD"
  skull: "eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA..."
  name: "Your Name"
```

#### Can I disable the confirmation GUI?

Yes! In `config.yml`:

```yaml
gui:
  confirm-selection-gui:
    enabled: false
```

This will apply message sets immediately when clicked.

#### How do I change GUI click actions?

In `config.yml`, customize what each click type does:

```yaml
gui:
  shift-left-click: favorite        # Options: favorite, select, remove-favorite, none
  shift-right-click: remove-favorite
  left-click: select
  right-click: select
```

#### Can I use messages in specific worlds only?

Yes! Use world restrictions in `config.yml`:

**Option 1: Enable in specific worlds**

```yaml
enabled-worlds:
  - world
  - world_survival
```

**Option 2: Disable in specific worlds**

```yaml
disabled-worlds:
  - world_nether
  - world_the_end
```

### 🎨 Customization

#### How do I use RGB colors?

BlockyJoin uses MiniMessage formatting. Use hex colors like this:

```yaml
join:
  - "<#FF0000>Red <#00FF00>Green <#0000FF>Blue"
```

#### How do I create color gradients?

Use the gradient tag:

```yaml
join:
  - "<gradient:#FF0000:#0000FF>Rainbow Text</gradient>"
```

#### How do I add hover text to messages?

Use hover events:

```yaml
join:
  - "<hover:show_text:'<white>Health: {player_health}<newline>Level: {player_level}'>{player}</hover> joined!"
```

When players hover over the name, they'll see the player's health and level.

#### Can I make messages clickable?

Yes! Use click events:

```yaml
join:
  - "<click:run_command:'/spawn'>Click to teleport!</click>"
  - "<click:open_url:'https://example.com'>Visit our website!</click>"
```

#### How do I use emojis in messages?

You can use Unicode emojis directly:

```yaml
join:
  - "🎮 {player} joined the game! 🎉"
```

### 🎯 Permissions

#### How do I give everyone access to the GUI?

```bash
/lp group default permission set blockyjoin.gui true
```

#### How do I give VIP access to specific message sets?

```bash
/lp group vip permission set blockyjoin.set.king true
/lp group vip permission set blockyjoin.set.wizard true
```

#### Can I make a message set available to everyone?

Yes! In `join-leave-messages.yml`, leave the permission field empty:

```yaml
your-set:
  permission: ""  # Available to all players
```

#### How do I restrict a message set to a specific rank?

Set a permission in `join-leave-messages.yml`:

```yaml
vip-set:
  permission: "blockyjoin.set.vip"
```

Then grant it to your rank:

```bash
/lp group vip permission set blockyjoin.set.vip true
```

### 📊 Database & Storage

#### Where are player selections stored?

Player selections are stored in an SQLite database at:

```
plugins/BlockyJoin/blockyjoin.db
```

#### How do I backup player data?

BlockyJoin automatically creates backups in:

```
plugins/BlockyJoin/backups/
```

To manually backup, simply copy the `blockyjoin.db` file.

#### Can I reset a player's message selection?

Yes, through the database or by deleting their entry and having them select again. For a fresh start, you can delete `blockyjoin.db` (creates a new empty database on restart).

#### What happens if the database gets corrupted?

BlockyJoin maintains automatic backups. Restore from:

```
plugins/BlockyJoin/backups/blockyjoin-backup-<date>.db
```

Simply rename a backup to `blockyjoin.db` and restart.

### 🔧 Troubleshooting

#### Messages aren't showing up

**Possible causes:**

1. Messages disabled in `config.yml` - Check `join.enabled: true`
2. World is disabled - Check `disabled-worlds` list
3. Plugin conflict - Disable other join message plugins
4. Wrong message type - Verify `message-type` setting

**Solutions:**

```yaml
# In config.yml
join:
  enabled: true
leave:
  enabled: true

# Remove world from disabled list
disabled-worlds: []
```

#### GUI not opening

**Possible causes:**

1. No permission - Player needs `blockyjoin.gui`
2. No message sets - Check `join-leave-messages.yml`
3. GUI error - Check console for errors

**Solutions:**

```bash
# Grant permission
/lp user <player> permission set blockyjoin.gui true

# Check console
/blockyjoin reload
```

#### Placeholders showing as text

**Problem:** `{player}` shows literally instead of player name

**Solutions:**

1. **Internal placeholders:** Use `{curly_braces}`
2. **PAPI placeholders:** Use `%percent_signs%`
3. **Verify spelling:** `{player}` not `{Player}`
4. **Test:** `/blockyjoin test join`

#### Colors not working

**Problem:** Colors show as text like `<#FF0000>`

**Solution:** Ensure you're using MiniMessage format correctly:

* ✅ Correct: `<#FF0000>Red Text`
* ❌ Wrong: `§cRed Text`
* ❌ Wrong: `&cRed Text`

#### Player heads showing as Steve

**Possible causes:**

1. Invalid base64 texture
2. Wrong material type
3. Quotes missing around skull value

**Solution:**

```yaml
item:
  material: "PLAYER_HEAD"  # Must be PLAYER_HEAD
  skull: "eyJ0ZXh0dXJl..."  # Must have quotes
```

#### Plugin won't load

**Possible causes:**

1. Wrong Java version (need Java 17+)
2. Wrong server software (need Paper/Folia)
3. Corrupted jar file

**Solutions:**

1. Check Java: `/version` - Should show Java 17+
2. Verify Paper/Folia
3. Re-download the jar file

### 💡 Features

#### Can players have multiple message sets?

Players can select one active message set at a time, but they can favorite multiple sets for quick access.

#### How does the favorites system work?

Players can:

1. Shift-click message sets to add/remove favorites
2. Open the favorites GUI to see only their favorited sets
3. Quickly switch between favorite sets

#### Can I have random messages?

Yes! Add multiple messages to a set:

```yaml
join:
  - "Message option 1"
  - "Message option 2"
  - "Message option 3"
```

BlockyJoin will randomly select one each time.

#### Can I test messages before using them?

Yes! Use:

```
/blockyjoin test join
/blockyjoin test leave
```

This shows you how your messages will look without broadcasting to the server.

#### Can I set different first-join messages?

Yes! Configure in `config.yml`:

```yaml
first-join:
  enabled: true
  random-message: true
  messages:
    - "Welcome {player} to the server!"
    - "Everyone welcome {player}!"
```

### 🔄 Updates

#### How do I update BlockyJoin?

1. Backup your server (recommended, but not required)
2. Stop your server
3. Replace the old jar with the new jar
4. Start your server
5. BlockyJoin will automatically update configs

#### Will updating delete my configs?

No! BlockyJoin's auto-updater:

* ✅ Preserves your custom settings
* ✅ Adds new default values
* ✅ Creates backups before updating
* ✅ Warns about deprecated options

#### Do I need to reset configs after updating?

Usually no. The auto-updater handles config migrations. Only reset if specifically instructed in update notes.

### 🎮 Gameplay

#### Can players preview messages before selecting?

Yes! The GUI shows preview placeholders `{join-preview}` and `{leave-preview}` in the item lore, displaying exactly how messages will look.

#### Can I reward players with message sets?

Yes! Grant permissions as rewards:

```bash
# Reward for completing quest
/lp user <player> permission set blockyjoin.set.dragon true

# Temporary reward (7 days)
/lp user <player> permission settemp blockyjoin.set.vip 7d
```

#### Can I create seasonal message sets?

Absolutely! BlockyJoin includes seasonal sets (Christmas, Halloween, Spring, Summer, Autumn, Winter). You can also:

1. Create custom seasonal sets
2. Grant temporary permissions during seasons
3. Use conditional permissions

#### Can I use BlockyJoin on BungeeCord?

BlockyJoin is a per-server plugin. Install it on each backend server where you want join/leave messages. However Proxy support is planned!

***

### Still Have Questions?

* 📚 Check the [Full Wiki](../blockyjoin.md)
* 💬 Join our [Discord](https://leafstudios.dev/discord)

---
description: >-
  Learn how to create your own unique join/leave message sets in
  join-leave-messages.yml.
---

# 🎨 Creating Message Sets

### 📖 Message Set Structure

Each message set follows this structure:

```yaml
messages:
  your-message-id:
    join:
      - "Join message line 1"
      - "Join message line 2 (optional)"
    leave:
      - "Leave message line 1"
      - "Leave message line 2 (optional)"
    item:
      material: "MATERIAL_NAME"
      skull: "base64_texture (optional)"
      name: "Display Name"
      lore:
        - "Lore line 1"
        - "Lore line 2"
    permission: "blockyjoin.set.your-message-id"
    glow-on-select: true
    info-lore:
      enabled: true
      selected-lore:
        - "Currently selected text"
      not-selected-lore:
        - "Click to select text"
    favorite-lore:
      enabled: true
      selected-icon:
        - "★"
      selected-lore:
        - "★ shift-click to add to favorites"
      not-selected-lore:
        - "★ shift-click to add to favorites"
```

### 🎯 Step-by-Step Guide

#### Step 1: Choose a Unique ID

The message ID should be:

* Lowercase
* No spaces (use hyphens instead)
* Descriptive of the theme

**Examples:**

* `king`
* `wizard`
* `space-explorer`
* `medieval-knight`

#### Step 2: Write Join Messages

```yaml
your-message-id:
  join:
    - "<#49FF00>[+] <white>{player} <#CB1FE9>has arrived!"
```

**Tips:**

* Use multiple lines for random selection
* Include placeholders like `{player}`, `{online}`, etc.
* Use MiniMessage for colors and formatting

**Multiple Messages (Random):**

```yaml
  join:
    - "<#49FF00>[+] <white>{player} <#CB1FE9>has arrived!"
    - "<#49FF00>[+] <#CB1FE9>{player} <white>joined the server!"
    - "<#49FF00>[+] <white>Welcome <#CB1FE9>{player}<white>!"
```

BlockyJoin will randomly select one message each time the player joins.

#### Step 3: Write Leave Messages

```yaml
  leave:
    - "<#FF3232>[-] <white>{player} <#CB1FE9>has left!"
```

Same principles as join messages apply here.

#### Step 4: Configure the GUI Item

**Basic Material**

```yaml
  item:
    material: "DIAMOND"
    name: "<#CB1FE9>My Custom Set"
    lore:
      - ""
      - "<white>A unique message set."
      - ""
```

**Custom Player Head**

For custom player heads, use base64 textures from [Minecraft-Heads.com](https://minecraft-heads.com/):

```yaml
  item:
    material: "PLAYER_HEAD"
    skull: "eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvNGU3YTY4ZDFkNTJiNzFhZjAxM2I4OGM4YmU1MDI4ZmYyNzNhMjA5ZTYyMmY1MDVlNDcyYzZiM2Q5ZDBlOTA1OSJ9fX0="
    name: "<#AC73D4>Wizard's Appearance"
    lore:
      - ""
      - "<white>Join/Leave messages with magic and mystery."
      - ""
```

**How to get skull textures:**

1. Visit https://minecraft-heads.com
2. Find a head you like
3. Click "Get Value"
4. Copy the value (starts with `eyJ...`)
5. Paste into the `skull:` field

**Preview Placeholders**

Use these placeholders in your lore to show message previews:

```yaml
    lore:
      - ""
      - "<white>Description of your message set."
      - ""
      - "<#CB1FE9>Preview:<reset>"
      - "{join-preview}"    # Shows actual join message
      - "{leave-preview}"   # Shows actual leave message
      - ""
      - "{info-lore}"       # Shows selection status
      - "{favorite-lore}"   # Shows favorite actions
```

#### Step 5: Set Permissions

```yaml
  permission: "blockyjoin.set.your-message-id"
```

* Leave **empty** (`permission: ""`) for free access to all players
* Set to a custom permission for restricted access
* Format: `blockyjoin.set.<id>`

#### Step 6: Configure Additional Options

**Glow Effect**

```yaml
  glow-on-select: true  # Item glows when selected
```

**Info Lore (Selection Status)**

```yaml
  info-lore:
    enabled: true
    selected-lore:
      - "<gold>ᴄᴜʀʀᴇɴᴛʟʏ ꜱᴇʟᴇᴄᴛᴇᴅ"
    not-selected-lore:
      - "<gold>ᴄʟɪᴄᴋ ᴛᴏ ꜱᴇʟᴇᴄᴛ"
```

**Favorite Lore**

```yaml
  favorite-lore:
    enabled: true
    selected-icon:
      - "<gold>★"
    selected-lore:
      - "<gold>★ shift-right-ᴄʟɪᴄᴋ ᴛᴏ ʀᴇᴍᴏᴠᴇ ꜰʀᴏᴍ ꜰᴀᴠᴏʀɪᴛᴇs"
    not-selected-lore:
      - "<gold>★ shift-ᴄʟɪᴄᴋ ᴛᴏ ᴀᴅᴅ ᴛᴏ ꜰᴀᴠᴏʀɪᴛᴇs"
```

### 📝 Complete Examples

#### Example 1: Simple Message Set

```yaml
messages:
  simple:
    join:
      - "<#49FF00>[+] {player}"
    leave:
      - "<#FF3232>[-] {player}"
    item:
      material: "PAPER"
      name: "<gray>Simple Messages"
      lore:
        - ""
        - "<white>Basic join/leave messages."
        - ""
        - "{info-lore}"
    permission: ""
    glow-on-select: true
    info-lore:
      enabled: true
      selected-lore:
        - "<gold>ᴄᴜʀʀᴇɴᴛʟʏ ꜱᴇʟᴇᴄᴛᴇᴅ"
      not-selected-lore:
        - "<gold>ᴄʟɪᴄᴋ ᴛᴏ ꜱᴇʟᴇᴄᴛ"
```

#### Example 2: VIP Message Set with Stats

```yaml
messages:
  vip-gold:
    join:
      - "<#FFD700>[VIP] {player} <white>has joined! <gray>({join_count} total joins)"
      - "<#FFD700>[VIP] <white>Welcome {player}! <gray>Online: {online}/{max_players}"
      - "<#FFD700>[VIP] {luckperms_prefix}{player} <white>entered the server!"
    leave:
      - "<#FF3232>[VIP] {player} <white>has left the server. Farewell!"
      - "<#FF3232>[VIP] <white>Goodbye {player}!"
    item:
      material: "GOLD_INGOT"
      name: "<#FFD700>VIP Gold Messages"
      lore:
        - ""
        - "<white>Exclusive VIP join/leave messages."
        - "<#FFD700>✦ <white>Shows your stats"
        - "<#FFD700>✦ <white>Gold formatting"
        - ""
        - "<#CB1FE9>Preview:<reset>"
        - "{join-preview}"
        - "{leave-preview}"
        - ""
        - "{info-lore}"
        - "{favorite-lore}"
    permission: "blockyjoin.set.vip-gold"
    glow-on-select: true
    info-lore:
      enabled: true
      selected-lore:
        - "<gold>ᴄᴜʀʀᴇɴᴛʟʏ ꜱᴇʟᴇᴄᴛᴇᴅ"
      not-selected-lore:
        - "<gold>ᴄʟɪᴄᴋ ᴛᴏ ꜱᴇʟᴇᴄᴛ"
    favorite-lore:
      enabled: true
      selected-icon:
        - "<gold>★"
      selected-lore:
        - "<gold>★ shift-right-ᴄʟɪᴄᴋ ᴛᴏ ʀᴇᴍᴏᴠᴇ ꜰʀᴏᴍ ꜰᴀᴠᴏʀɪᴛᴇs"
      not-selected-lore:
        - "<gold>★ shift-ᴄʟɪᴄᴋ ᴛᴏ ᴀᴅᴅ ᴛᴏ ꜰᴀᴠᴏʀɪᴛᴇs"
```

#### Example 3: Themed Message Set with Custom Head

```yaml
messages:
  space-explorer:
    join:
      - "<#49FF00>[+] <white>Astronaut <#00BFFF>{player} <white>has landed on the station!"
      - "<#49FF00>[+] <white>Mission Control: <#00BFFF>{player} <white>has docked successfully!"
      - "<#49FF00>[+] <#00BFFF>{player} <white>floated through the airlock!"
    leave:
      - "<#FF3232>[-] <white>Astronaut <#00BFFF>{player} <white>has launched into space!"
      - "<#FF3232>[-] <#00BFFF>{player} <white>returned to the mothership!"
      - "<#FF3232>[-] <white>Mission Control: <#00BFFF>{player} <white>is off the station."
    item:
      material: "PLAYER_HEAD"
      skull: "eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvNGVjZmQ0MTkzN2E3ZjI3MjNhNGNjYTVhM2JmNDkzNzdhMzRjNDNjNGJhZTQzODMzMzJiOGQ3NTUwMGQzYjcifX19"
      name: "<#00BFFF>Space Explorer"
      lore:
        - ""
        - "<white>Join/leave messages for space adventurers."
        - ""
        - "<#00BFFF>🚀 <white>Explore the cosmos"
        - "<#00BFFF>🌟 <white>Journey among the stars"
        - ""
        - "<#CB1FE9>Preview:<reset>"
        - "{join-preview}"
        - "{leave-preview}"
        - ""
        - "{info-lore}"
        - "{favorite-lore}"
    permission: "blockyjoin.set.space-explorer"
    glow-on-select: true
    info-lore:
      enabled: true
      selected-lore:
        - "<gold>ᴄᴜʀʀᴇɴᴛʟʏ ꜱᴇʟᴇᴄᴛᴇᴅ"
      not-selected-lore:
        - "<gold>ᴄʟɪᴄᴋ ᴛᴏ ꜱᴇʟᴇᴄᴛ"
    favorite-lore:
      enabled: true
      selected-icon:
        - "<gold>★"
      selected-lore:
        - "<gold>★ shift-right-ᴄʟɪᴄᴋ ᴛᴏ ʀᴇᴍᴏᴠᴇ ꜰʀᴏᴍ ꜰᴀᴠᴏʀɪᴛᴇs"
      not-selected-lore:
        - "<gold>★ shift-ᴄʟɪᴄᴋ ᴛᴏ ᴀᴅᴅ ᴛᴏ ꜰᴀᴠᴏʀɪᴛᴇs"
```

### 🎨 MiniMessage Formatting

BlockyJoin uses MiniMessage for advanced text formatting. Here are some examples:

#### Colors

```yaml
# Hex colors
<#CB1FE9>Purple Text
<#49FF00>Green Text

# Gradients
<gradient:#FF0000:#00FF00>Rainbow Text</gradient>

# Named colors
<red>Red Text
<blue>Blue Text
<gold>Gold Text
```

#### Text Decoration

```yaml
<bold>Bold Text
<italic>Italic Text
<underlined>Underlined Text
<strikethrough>Strikethrough Text
<obfuscated>Magic Text
```

#### Hover Events

```yaml
<hover:show_text:'<white>This appears when you hover!'>Hover over me!</hover>
```

#### Click Events

```yaml
<click:run_command:'/warp spawn'>Click to teleport!</click>
<click:open_url:'https://example.com'>Visit our website!</click>
```

#### Combined Example

```yaml
join:
  - "<gradient:#49FF00:#CB1FE9>[+]</gradient> <hover:show_text:'<white>Health: {player_health}<newline>Level: {player_level}'><bold>{player}</bold></hover> <white>has joined!"
```

### 🛠️ Best Practices

#### DO:

✅ Use meaningful IDs (`medieval-knight` not `set1`)\
✅ Include preview placeholders in lore\
✅ Test messages with `/blockyjoin test`\
✅ Use multiple message variants for variety\
✅ Set appropriate permissions\
✅ Use custom player heads for unique themes\
✅ Keep messages concise and readable

#### DON'T:

❌ Use spaces in message IDs\
❌ Make messages too long (>80 characters recommended)\
❌ Forget to reload after changes\
❌ Use invalid material names\
❌ Leave skull field for non-PLAYER\_HEAD materials\
❌ Use uppercase in IDs\
❌ Duplicate message IDs

### 🔄 Testing Your Message Set

#### 1. Save your changes

Edit `join-leave-messages.yml` and save the file.

#### 2. Reload the plugin

```
/blockyjoin reload
```

#### 3. Test the messages

```
/blockyjoin set your-message-id
/blockyjoin test join
/blockyjoin test leave
```

#### 4. View in GUI

```
/blockyjoin
```

Check that your message set appears with correct icon, name, and lore.

### 🐛 Troubleshooting

#### Message Set Not Showing

**Problem:** New message set doesn't appear in GUI

**Solutions:**

1. Check for YAML syntax errors
2. Ensure message ID is unique
3. Reload plugin: `/blockyjoin reload`
4. Check console for errors

#### Invalid Material

**Problem:** GUI item shows as barrier/error

**Solutions:**

1. Verify material name: [Spigot Materials](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/Material.html)
2. Use UPPER\_CASE format: `DIAMOND_SWORD`
3. Reload after fixing

#### Skull Not Loading

**Problem:** Player head shows as Steve head

**Solutions:**

1. Ensure `material: "PLAYER_HEAD"`
2. Verify base64 is complete (starts with `eyJ`)
3. Test texture on https://minecraft-heads.com
4. Check for quotes around skull value

#### Colors Not Working

**Problem:** Colors show as text

**Solutions:**

1. Use MiniMessage format: `<#HEX>` not `§c`
2. Check for typos in color codes
3. Ensure format is `<#RRGGBB>`

#### Placeholders Not Replacing

**Problem:** `{player}` shows literally

**Solutions:**

1. Check placeholder spelling
2. Ensure using curly braces `{}`
3. For PAPI, use percent signs `%player_name%`
4. Test with `/blockyjoin test join`

### 📚 Additional Resources

* [MiniMessage Documentation](https://docs.advntr.dev/minimessage/format.html)
* [Minecraft Heads Database](https://minecraft-heads.com/)
* [Material List](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/Material.html)
* [Placeholders Guide](placeholders.md)
* [GUI Customization](gui-customization.md)

***

**Next Steps:**

* [Learn About Placeholders](placeholders.md)
* [Set Up Permissions](commands-and-permissions.md)


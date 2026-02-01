---
description: >-
  BlockyJoin supports a wide variety of placeholders for customizing your
  join/leave messages.
---

# 🏷️ Placeholders

### 📌 Internal Placeholders

These placeholders are built into BlockyJoin and work without any additional plugins.

#### Player Placeholders

| Placeholder            | Description           | Example Output |
| ---------------------- | --------------------- | -------------- |
| `{player}`             | Player's username     | `Steve`        |
| `{player_name}`        | Player's username     | `Steve`        |
| `{player_displayname}` | Player's display name | `§6Steve`      |
| `{player_uuid}`        | Player's UUID         | `069a79f4-...` |

#### Player Stats

| Placeholder           | Description             | Example Output |
| --------------------- | ----------------------- | -------------- |
| `{player_health}`     | Current health          | `20.0`         |
| `{player_max_health}` | Maximum health          | `20.0`         |
| `{player_food}`       | Food level              | `20`           |
| `{player_level}`      | Experience level        | `30`           |
| `{player_exp}`        | Experience progress     | `0.45`         |
| `{join_count}`        | Times player has joined | `142`          |
| `{player_joins}`      | Times player has joined | `142`          |

#### Player Location

| Placeholder    | Description  | Example Output |
| -------------- | ------------ | -------------- |
| `{player_x}`   | X coordinate | `1250`         |
| `{player_y}`   | Y coordinate | `64`           |
| `{player_z}`   | Z coordinate | `-897`         |
| `{world}`      | World name   | `world`        |
| `{world_name}` | World name   | `world`        |

#### Server Placeholders

| Placeholder        | Description         | Example Output |
| ------------------ | ------------------- | -------------- |
| `{online}`         | Online player count | `42`           |
| `{online_players}` | Online player count | `42`           |
| `{max_players}`    | Max player count    | `100`          |

#### GUI Placeholders

| Placeholder      | Description      | Example Output |
| ---------------- | ---------------- | -------------- |
| `{current_page}` | Current GUI page | `1`            |
| `{total_pages}`  | Total GUI pages  | `3`            |
| `{max_pages}`    | Total GUI pages  | `3`            |

### 🔌 LuckPerms Integration

If LuckPerms is installed, these additional placeholders become available:

| Placeholder          | Description      | Example Output |
| -------------------- | ---------------- | -------------- |
| `{player_prefix}`    | LuckPerms prefix | `§c[Admin]§r`  |
| `{luckperms_prefix}` | LuckPerms prefix | `§c[Admin]§r`  |
| `{player_suffix}`    | LuckPerms suffix | `§7[VIP]§r`    |
| `{luckperms_suffix}` | LuckPerms suffix | `§7[VIP]§r`    |
| `{player_group}`     | Primary group    | `admin`        |
| `{luckperms_group}`  | Primary group    | `admin`        |

### 🎨 PlaceholderAPI Support

BlockyJoin fully supports [PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/), allowing you to use thousands of placeholders from other plugins.

#### Installation

1. Download PlaceholderAPI from SpigotMC
2. Place in `plugins` folder
3. Restart server
4. Download expansion packs: `/papi ecloud download <expansion>`

#### Popular PAPI Expansions

**Player Expansions**

```
%player_name% - Player name
%player_displayname% - Display name
%player_uuid% - Player UUID
%player_ping% - Player ping
%player_level% - Player level
%player_exp% - Player experience
%player_health% - Player health
%player_max_health% - Max health
%player_food_level% - Hunger level
%player_gamemode% - Game mode
%player_world% - Current world
```

**Vault (Economy)**

```
%vault_eco_balance% - Player balance
%vault_eco_balance_formatted% - Formatted balance
%vault_rank% - Player rank
%vault_prefix% - Chat prefix
%vault_suffix% - Chat suffix
```

**Statistics**

```
%statistic_deaths% - Death count
%statistic_kills% - Kill count
%statistic_play_time% - Play time
%statistic_player_kills% - Player kills
%statistic_mob_kills% - Mob kills
```

**Custom Placeholders**

```
%server_name% - Server name
%server_tps% - Server TPS
%server_online% - Online players
%server_max_players% - Max players
%server_unique_joins% - Unique joins
```

#### Example Usage

```yaml
# In join-leave-messages.yml
vip:
  join:
    - "<#49FF00>[+] %vault_prefix% {player} <gray>| <white>Balance: <gold>$%vault_eco_balance_formatted%"
    - "<#49FF00>[+] <white>%luckperms_prefix% {player} <gray>has joined! <white>Ping: %player_ping%ms"
  leave:
    - "<#FF3232>[-] %vault_prefix% {player} <gray>played for %statistic_time_played%"
```

### 🎯 Message Set Preview Placeholders

These special placeholders are used in GUI item lore for previewing messages:

| Placeholder       | Description                                    |
| ----------------- | ---------------------------------------------- |
| `{join-preview}`  | Shows a preview of the join message            |
| `{leave-preview}` | Shows a preview of the leave message           |
| `{info-lore}`     | Shows selection status (selected/not selected) |
| `{favorite-lore}` | Shows favorite status and action hint          |
| `{favorite-icon}` | Shows ★ icon if message set is favorited       |

#### Example in join-leave-messages.yml

```yaml
wizard:
  item:
    lore:
      - ""
      - "<white>Join/Leave messages with magic and mystery."
      - ""
      - "<#CB1FE9>Preview:<reset>"
      - "{join-preview}"    # Automatically replaced with actual join message
      - "{leave-preview}"   # Automatically replaced with actual leave message
      - ""
      - "{info-lore}"       # Shows "ᴄʟɪᴄᴋ ᴛᴏ ꜱᴇʟᴇᴄᴛ" or "ᴄᴜʀʀᴇɴᴛʟʏ ꜱᴇʟᴇᴄᴛᴇᴅ"
      - "{favorite-lore}"   # Shows favorite action hints
```

### 💡 Practical Examples

#### Example 1: Basic Join Message

```yaml
join:
  - "<#49FF00>[+] {player} <white>joined the server!"
```

**Output:** `[+] Steve joined the server!`

***

#### Example 2: With Stats

```yaml
join:
  - "<#49FF00>[+] {player} <gray>has joined <white>({join_count} total joins)"
```

**Output:** `[+] Steve has joined (142 total joins)`

***

#### Example 3: With LuckPerms

```yaml
join:
  - "<#49FF00>[+] {luckperms_prefix} {player} {luckperms_suffix}"
```

**Output:** `[+] [Admin] Steve [VIP]`

***

#### Example 4: With PlaceholderAPI

```yaml
join:
  - "<#49FF00>[+] {player} <gray>| <white>Balance: <gold>$%vault_eco_balance% <gray>| <white>Ping: %player_ping%ms"
```

**Output:** `[+] Steve | Balance: $50,000 | Ping: 23ms`

***

#### Example 5: With World Info

```yaml
join:
  - "<#49FF00>[+] {player} <white>joined in <#CB1FE9>{world} <white>at <#CB1FE9>({player_x}, {player_y}, {player_z})"
```

**Output:** `[+] Steve joined in world at (1250, 64, -897)`

***

#### Example 6: Complex Multi-Placeholder

```yaml
join:
  - "<#49FF00>[+] %vault_prefix%{player}%vault_suffix% <gray>[<white>Lvl {player_level}<gray>] <white>joined <#CB1FE9>{world} <gray>(<white>{online}<gray>/<white>{max_players}<gray>)"
```

**Output:** `[+] [Admin]Steve[VIP] [Lvl 30] joined world (42/100)`

***

### 🔧 Creating Dynamic Messages

#### Random Messages with Placeholders

```yaml
join:
  - "<#49FF00>[+] {player} <white>arrived! <gray>(Join #{join_count})"
  - "<#49FF00>[+] <white>Welcome {player}! <gray>You've joined {join_count} times!"
  - "<#49FF00>[+] {player} <white>is here! <gray>Total joins: {join_count}"
```

Each time a player joins, BlockyJoin randomly selects one message and replaces all placeholders.

#### Conditional Formatting with MiniMessage

```yaml
join:
  - "<#49FF00>[+] <hover:show_text:'<white>Health: {player_health}/{player_max_health}<newline>Level: {player_level}<newline>World: {world}'>{player}</hover> <white>joined!"
```

Creates hover text showing player stats when you hover over their name.

### 📋 Placeholder Priority

When multiple placeholder systems are available, they are processed in this order:

1. **Message Set Placeholders** (`{join-preview}`, `{leave-preview}`, etc.)
2. **Internal Placeholders** (`{player}`, `{online}`, etc.)
3. **Custom Placeholders** (passed via plugin API or specific contexts)
4. **PlaceholderAPI** (if installed) - `%placeholder%`
5. **MiniMessage Formatting** (colors, hover, click events)

### ⚠️ Important Notes

#### Placeholder Format

* **Internal & LuckPerms**: Use `{curly_braces}`
* **PlaceholderAPI**: Use `%percent_signs%`

#### Case Sensitivity

* Placeholders are **case-sensitive**
* `{player}` works, `{PLAYER}` does not
* `%player_name%` works, `%PLAYER_NAME%` does not

#### Placeholder Testing

Use `/blockyjoin test join` or `/blockyjoin test leave` to see how your placeholders render in real-time.

#### Missing Placeholders

If a placeholder is not found:

* Internal placeholders return empty string
* PAPI placeholders return the placeholder itself (e.g., `%unknown_placeholder%`)

#### Performance

* Internal placeholders are fastest
* LuckPerms placeholders are fast
* PAPI placeholders may be slower depending on the expansion

### 🔍 Debugging Placeholders

#### Check Available Placeholders

```
/papi ecloud placeholders <expansion>
```

#### Test PlaceholderAPI

```
/papi parse me {your_message_here}
```

#### Check LuckPerms Data

```
/lp user <player> info
```

***

**See Also:**

* [Creating Message Sets](creating-message-sets.md)
* [MiniMessage Format](https://docs.advntr.dev/minimessage/format.html)
* [PlaceholderAPI Wiki](https://github.com/PlaceholderAPI/PlaceholderAPI/wiki)

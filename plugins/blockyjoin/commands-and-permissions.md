---
description: Complete reference for all BlockyJoin commands and permissions.
---

# 🔑 Commands & Permissions

### 📜 Commands

#### Main Command

```
/blockyjoin [subcommand]
```

**Aliases:** `/bj`, `/joinmsg`

#### Subcommands

| Command                          | Description                       | Permission          | Aliases           |
| -------------------------------- | --------------------------------- | ------------------- | ----------------- |
| `/blockyjoin`                    | Opens the message selection GUI   | `blockyjoin.gui`    | `/bj`, `/joinmsg` |
| `/blockyjoin reload`             | Reloads all configuration files   | `blockyjoin.reload` | -                 |
| `/blockyjoin set <id>`           | Sets your message set via command | `blockyjoin.set`    | -                 |
| `/blockyjoin test <join\|leave>` | Tests your current messages       | `blockyjoin.test`   | -                 |
| `/blockyjoin list`               | Lists all available message sets  | `blockyjoin.list`   | -                 |
| `/blockyjoin help`               | Shows help menu                   | -                   | `?`               |

#### Command Details

**`/blockyjoin`**

Opens the interactive GUI for selecting join/leave message sets.

**Usage:**

```
/blockyjoin
```

**Example:**

```
Player runs: /blockyjoin
Opens GUI showing all available message sets
```

***

**`/blockyjoin reload`**

Reloads all configuration files without restarting the server.

**Usage:**

```
/blockyjoin reload
```

**What gets reloaded:**

* `config.yml`
* `messages.yml`
* `menu.yml`
* `join-leave-messages.yml`

**Note:** Database connections are maintained during reload.

***

**`/blockyjoin set <id>`**

Directly sets your message set without opening the GUI.

**Usage:**

```
/blockyjoin set <message-set-id>
```

**Arguments:**

* `<message-set-id>` - The ID of the message set (e.g., `king`, `pirate`, `wizard`)

**Examples:**

```
/blockyjoin set king
/blockyjoin set wizard
/blockyjoin set default
```

**Requirements:**

* Player must have permission for the specific message set
* Message set must exist in `join-leave-messages.yml`

***

**`/blockyjoin test <type>`**

Tests your currently selected join or leave message.

**Usage:**

```
/blockyjoin test <join|leave>
```

**Arguments:**

* `join` - Tests join message
* `leave` - Tests leave message

**Examples:**

```
/blockyjoin test join
/blockyjoin test leave
```

**Note:** Only you will see the test message. This is useful for previewing how your messages will look before using them live.

***

**`/blockyjoin list`**

Displays a list of all available message sets in chat.

**Usage:**

```
/blockyjoin list
```

**Output Example:**

```
─+──────────────[ BlockyJoin ]──────────────+─
Available Message Sets:
» default (Default Messages)
» king (King's Entrance)
» pirate (Pirate's Arrival)
» wizard (Wizard's Appearance)
─+─────────────────────────────────────────+─
```

### 🔐 Permissions

#### Core Permissions

| Permission          | Description                         | Default |
| ------------------- | ----------------------------------- | ------- |
| `blockyjoin.*`      | Grants all BlockyJoin permissions   | `op`    |
| `blockyjoin.use`    | Basic plugin access                 | `true`  |
| `blockyjoin.gui`    | Access to the message selection GUI | `true`  |
| `blockyjoin.reload` | Reload configuration files          | `op`    |
| `blockyjoin.set`    | Use `/blockyjoin set` command       | `true`  |
| `blockyjoin.test`   | Test join/leave messages            | `op`    |
| `blockyjoin.list`   | List available message sets         | `op`    |
| `blockyjoin.admin`  | Admin commands and database access  | `op`    |

#### Message Set Permissions

Each message set has its own permission node, allowing fine-grained control over who can use which messages.

**Format:** `blockyjoin.set.<message-set-id>`

**Default Message Sets:**

| Permission                     | Message Set          |
| ------------------------------ | -------------------- |
| `blockyjoin.set.default`       | Default Messages     |
| `blockyjoin.set.king`          | King's Entrance      |
| `blockyjoin.set.princess`      | Princess's Entrance  |
| `blockyjoin.set.pirate`        | Pirate's Arrival     |
| `blockyjoin.set.wizard`        | Wizard's Appearance  |
| `blockyjoin.set.fantasy`       | Fantasy Approach     |
| `blockyjoin.set.ninja`         | Ninja's Stealth      |
| `blockyjoin.set.astronaut`     | Astronaut's Landing  |
| `blockyjoin.set.cowboy`        | Cowboy's Ride        |
| `blockyjoin.set.superhero`     | Superhero's Entry    |
| `blockyjoin.set.robot`         | Robot's Boot         |
| `blockyjoin.set.vampire`       | Vampire's Rise       |
| `blockyjoin.set.ghost`         | Ghost's Haunt        |
| `blockyjoin.set.angel`         | Angel's Descent      |
| `blockyjoin.set.demon`         | Demon's Arrival      |
| `blockyjoin.set.dragon`        | Dragon's Flight      |
| `blockyjoin.set.phoenix`       | Phoenix's Rebirth    |
| `blockyjoin.set.unicorn`       | Unicorn's Gallop     |
| `blockyjoin.set.mermaid`       | Mermaid's Swim       |
| `blockyjoin.set.alien`         | Alien's Touchdown    |
| `blockyjoin.set.time-traveler` | Time Traveler's Warp |
| `blockyjoin.set.detective`     | Detective's Case     |
| `blockyjoin.set.chef`          | Chef's Kitchen       |
| `blockyjoin.set.artist`        | Artist's Canvas      |
| `blockyjoin.set.musician`      | Musician's Melody    |
| `blockyjoin.set.scientist`     | Scientist's Lab      |
| `blockyjoin.set.explorer`      | Explorer's Trek      |
| `blockyjoin.set.gamer`         | Gamer's Spawn        |
| `blockyjoin.set.streamer`      | Streamer's Live      |
| `blockyjoin.set.christmas`     | Christmas's Joy      |
| `blockyjoin.set.halloween`     | Halloween's Haunting |
| `blockyjoin.set.spring`        | Spring's Bloom       |
| `blockyjoin.set.summer`        | Summer's Splash      |
| `blockyjoin.set.autumn`        | Autumn's Harvest     |
| `blockyjoin.set.winter`        | Winter's Frost       |
| `blockyjoin.set.batman`        | Batman's Return      |

### 🔒 Permission Checking

#### In GUI

When a player opens the GUI:

1. Plugin checks `blockyjoin.set.<id>` for each message set
2. Message sets without permission show a "no permission" indicator
3. Players can still see locked sets (preview) but cannot select them

#### In Commands

When using `/blockyjoin set <id>`:

1. Plugin checks if player has `blockyjoin.set.<id>`
2. If no permission: Shows error message
3. If has permission: Sets message and confirms

Prevent specific players from using certain sets:

Prevent specific players from using certain sets:

### 📝 Notes

* **Default Set**: If no message set permission is specified in `join-leave-messages.yml`, the set is available to all players
* **Multiple Sets**: Players can have permission for multiple sets and switch between them
* **GUI Access**: `blockyjoin.gui` permission is required to open the selection menu
* **Admin Commands**: Some admin commands require `blockyjoin.admin` or `blockyjoin.reload`

***

**See Also:**

* [Creating Message Sets](creating-message-sets.md)
* [GUI Customization](gui-customization.md)

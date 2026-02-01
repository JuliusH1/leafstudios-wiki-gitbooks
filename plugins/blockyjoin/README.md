---
description: >-
  Welcome to the official BlockyJoin documentation! This wiki provides
  comprehensive information about installing, configuring, and using BlockyJoin.
cover: ../../.gitbook/assets/BlockyJoin (3).png
coverY: 0
coverHeight: 382
layout:
  width: default
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
---

# 👋 BlockyJoin

### 🎯 What is BlockyJoin?

BlockyJoin is a premium, high-performance join/leave message plugin for Paper/Folia servers (1.21+). It offers fully customizable message sets, beautiful GUIs, database storage, and Folia compatibility. Give your players the ability to choose their own unique join/leave messages with permission-based access.

### ✨ Key Features

* **Highly Configurable** - Customize every aspect of join/leave messages
* **MiniMessage Support** - Full RGB gradients, hover events, and modern formatting
* **Customizable GUI** - Beautiful, paginated message selection interface with favorites
* **Permission-Based Message Sets** - Control access to different message themes
* **Folia Compatible** - Optimized for Folia's regionized scheduling system
* **Database Storage** - SQLite database for player preferences with automatic backup system
* **Multiple Message Types** - CHAT, TITLE, ACTIONBAR support
* **World-Based Messages** - Enable/disable messages in specific worlds
* **First Join Messages** - Special messages for new players
* **Preview System** - Test messages before applying them
* **Multi-Page GUI** - Handle unlimited message sets with pagination
* **Skull Texture Support** - Custom player heads for GUI items
* **PlaceholderAPI Integration** - Support for PAPI placeholders in messages
* **LuckPerms Integration** - Display prefixes, suffixes, and groups in messages
* **Auto-Config Updates** - Seamlessly updates configs while preserving your settings

### 📚 Quick Links

#### Getting Started

* [Installation Guide](installation-and-setup.md)
* [Quick Start Guide](quick-start.md)
* [Commands & Permissions](commands-and-permissions.md)

#### Advanced

* [Placeholders](placeholders.md)
* [Creating Custom Message Sets](creating-message-sets.md)
* [GUI Customization](gui-customization.md)

#### Support

* [FAQ](frequently-asked-questions.md)
* [Troubleshooting](frequently-asked-questions.md#troubleshooting)

### 🎮 Quick Example

```yaml
# Example message set from join-leave-messages.yml
wizard:
  join:
    - "<#49FF00>[+] <white>Wizard <#AC73D4>{player} <white>appeared in a puff of smoke!"
  leave:
    - "<#FF3232>[-] <white>Wizard <#AC73D4>{player} <white>vanished in smoke."
  item:
    material: "PLAYER_HEAD"
    name: "<#AC73D4>Wizard's Appearance"
  permission: "blockyjoin.set.wizard"
```

### 💬 Support & Community

* **Discord**: [Join our Discord](https://leafstudios.dev/discord)
* **Documentation**: You're here!

### 📝 Credits

**Developed by:**

* Leaf Studios
* Blockyware Studios

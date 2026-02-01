---
description: Complete guide to customizing the BlockyJoin GUI appearance and behavior.
---

# 🎨 GUI Customization

### 📖 Overview

BlockyJoin provides three customizable GUIs:

1. **Main GUI** - Primary message selection interface
2. **Confirmation GUI** - Optional confirmation screen
3. **Favorites GUI** - Quick access to favorited message sets

The Config files provided have a custom style created by our collaborator [Blockyware](https://builtbybit.com/creators/blockyware.241768/).

All GUI settings are configured in `menu.yml`.

### 🎨 Main GUI Configuration

#### Basic Settings

```yaml
main-gui:
  title: "   <st>     <reset> <dark_gray>SELECT A MESSAGE <st>     "
  rows: 6
  close-on-select: false
  always-show-navigation: true
```

**Title**

The GUI title supports MiniMessage formatting:

```yaml
# Examples:
title: "<gradient:#FF0000:#0000FF>Message Selection</gradient>"
title: "<#CB1FE9><bold>BlockyJoin Menu</bold>"
title: "   <st>═════<reset> <gold>Choose Your Style <st>═════   "
```

**Tips:**

* Use `<st>` for strikethrough decorative lines
* Keep under 32 characters for best appearance
* Test in-game to verify formatting

**Rows**

Number of rows in the GUI (1-6):

```yaml
rows: 6  # Creates a 54-slot GUI (6 rows × 9 slots)
rows: 5  # Creates a 45-slot GUI (5 rows × 9 slots)
rows: 4  # Creates a 36-slot GUI (4 rows × 9 slots)
```

**Recommended:** 6 rows for maximum message sets per page

**Close on Select**

Controls whether the GUI closes after selecting a message set:

```yaml
close-on-select: true   # GUI closes after selection
close-on-select: false  # GUI stays open after selection
```

**Recommended:** `false` - Allows players to browse and compare sets

**Always Show Navigation**

Controls whether navigation buttons appear when not needed:

```yaml
always-show-navigation: true   # Shows previous/next on all pages
always-show-navigation: false  # Hides buttons on first/last page
```

#### Message Set Slots

Define where message set items appear in the GUI:

```yaml
join-leave-message-slots:
  - "10-16"  # Row 2, slots 2-8
  - "19-25"  # Row 3, slots 2-8
  - "28-34"  # Row 4, slots 2-8
```

**Format:** `"start-end"` for slot ranges

**Slot Numbering:**

```
Row 1: 0  1  2  3  4  5  6  7  8
Row 2: 9 10 11 12 13 14 15 16 17
Row 3: 18 19 20 21 22 23 24 25 26
Row 4: 27 28 29 30 31 32 33 34 35
Row 5: 36 37 38 39 40 41 42 43 44
Row 6: 45 46 47 48 49 50 51 52 53
```

**Examples:**

```yaml
# Center 3×3 grid
join-leave-message-slots:
  - "12-14"
  - "21-23"
  - "30-32"

# Full rows
join-leave-message-slots:
  - "9-17"   # Entire row 2
  - "18-26"  # Entire row 3
  - "27-35"  # Entire row 4

# Compact layout
join-leave-message-slots:
  - "10-16"
  - "19-25"
```

### 🎯 GUI Items

#### Filler Items

Create decorative borders and backgrounds:

```yaml
items:
  fill1:
    slot: 0-9              # Top row
    material: "BLACK_STAINED_GLASS_PANE"
    name: " "              # Blank name
    lore: []               # No lore
```

**Common Filler Patterns:**

```yaml
# Border only
items:
  top-border:
    slot: 0-8
    material: "BLACK_STAINED_GLASS_PANE"
    name: " "
  
  bottom-border:
    slot: 45-53
    material: "BLACK_STAINED_GLASS_PANE"
    name: " "
  
  left-border:
    slot: 9,18,27,36
    material: "BLACK_STAINED_GLASS_PANE"
    name: " "
  
  right-border:
    slot: 17,26,35,44
    material: "BLACK_STAINED_GLASS_PANE"
    name: " "

# Decorative pattern
items:
  accent1:
    slot: 46-47
    material: "MAGENTA_STAINED_GLASS_PANE"
    name: " "
  
  accent2:
    slot: 51-53
    material: "MAGENTA_STAINED_GLASS_PANE"
    name: " "
```

#### Navigation Buttons

**Next Page Button**

```yaml
next-page:
  slot: 50
  material: "PLAYER_HEAD"
  skull: "eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvNjgyYWQxYjljYjRkZDIxMjU5YzBkNzVhYTMxNWZmMzg5YzNjZWY3NTJiZTM5NDkzMzgxNjRiYWM4NGE5NmUifX19"
  name: "<reset><#49FF00>Next Page"
  lore:
    - "<reset><white>Click to view. {current_page}/{total_pages}"
  action: "[next_page]"
```

**Available Actions:**

* `[next_page]` - Go to next page
* `[previous_page]` - Go to previous page
* `[close]` - Close the GUI
* `[open_favorites]` - Open favorites GUI
* `[back]` - Return to main GUI (from favorites)

**Custom Arrows:**

```yaml
# Green arrow (next)
next-page:
  material: "LIME_DYE"
  name: "<green>→ Next Page"

# Red arrow (previous)  
previous-page:
  material: "RED_DYE"
  name: "<red>← Previous Page"

# Book style
next-page:
  material: "BOOK"
  name: "<gold>Next →"
```

**Previous Page Button**

```yaml
previous-page:
  slot: 48
  material: "PLAYER_HEAD"
  skull: "eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvMzdhZWU5YTc1YmYwZGY3ODk3MTgzMDE1Y2NhMGIyYTdkNzU1YzYzMzg4ZmYwMTc1MmQ1ZjQ0MTlmYzY0NSJ9fX0="
  name: "<reset><#49FF00>Previous Page"
  lore:
    - "<reset><white>Click to view. {current_page}/{total_pages}"
  action: "[previous_page]"
```

**Close Button**

```yaml
close-button:
  slot: 49
  material: "PLAYER_HEAD"
  skull: "eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvYmViNTg4YjIxYTZmOThhZDFmZjRlMDg1YzU1MmRjYjA1MGVmYzljYWI0MjdmNDYwNDhmMThmYzgwMzQ3NWY3In19fQ=="
  name: "<reset><#FF3232>Close"
  lore:
    - "<reset><white>Click to close."
  action: "[close]"
```

**Alternative Styles:**

```yaml
# Barrier style
close-button:
  material: "BARRIER"
  name: "<red><bold>✖ Close"
  
# Redstone style
close-button:
  material: "REDSTONE_BLOCK"
  name: "<red>Close Menu"

# Door style
close-button:
  material: "OAK_DOOR"
  name: "<gray>Exit"
```

**Favorites Button**

```yaml
favorites-button:
  slot: 45
  material: "GOLD_INGOT"
  name: "<reset><#FFD700>★ Favorites"
  lore:
    - "<reset><white>Click to view your favorited message sets."
  action: "[open_favorites]"
```

**Alternative Styles:**

```yaml
# Star item
favorites-button:
  material: "NETHER_STAR"
  name: "<gold>★ Favorites"

# Book style
favorites-button:
  material: "ENCHANTED_BOOK"
  name: "<gold>Favorite Sets"

# Heart style
favorites-button:
  material: "PLAYER_HEAD"
  skull: "eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvNDU5Nzk3MWQ4OTU5NmNiNGY0M2U0OTVkYjgzOGY2NGZlMTQ4NDk5YTM0YzE5NGM2MmU5YjM2NDczZGFhNTJiIn19fQ=="
  name: "<red>❤ Favorites"
```

### 🎯 Confirmation GUI

Optional confirmation screen that appears after clicking a message set:

```yaml
confirmation-gui:
  title: "   <st>     <reset> <dark_gray>SELECT A MESSAGE <st>     "
  rows: 3

  filler-item:
    material: "BLACK_STAINED_GLASS_PANE"
    name: " "
    lore: []
```

#### Confirm Button

```yaml
items:
  confirm:
    slot: 11
    material: "LIME_STAINED_GLASS_PANE"
    name: "<reset><#49FF00>Confirm Selection"
    lore:
      - "<reset><white>Click to confirm your message set selection."
    action: "[confirm_selection]"
```

**Alternative Styles:**

```yaml
# Checkmark style
confirm:
  material: "LIME_WOOL"
  name: "<green>✔ Confirm"

# Emerald style
confirm:
  material: "EMERALD_BLOCK"
  name: "<green>Confirm"
```

#### Selected Message Set Display

Shows preview of the selected message set:

```yaml
selected-message-set:
  slot: 13
  material: "BOOK"
  name: "<reset><#CB1FE9>Selected Message Set"
  lore:
    - "<reset><white>You have selected the following message set:"
    - ""
    - "<reset><gold>{message_set_name}"
    - ""
    - "<reset><white>Join Message:"
    - "<reset><gray>{join-preview}"
    - ""
    - "<reset><white>Leave Message:"
    - "<reset><gray>{leave-preview}"
    - ""
    - "<reset><white>Click confirm to apply this message set."
```

**Placeholders:**

* `{message_set_name}` - Name of the selected set
* `{join-preview}` - Preview of join message
* `{leave-preview}` - Preview of leave message

#### Cancel Button

```yaml
cancel:
  slot: 15
  material: "RED_STAINED_GLASS_PANE"
  name: "<reset><#FF3232>Cancel Selection"
  lore:
    - "<reset><white>Click to cancel and return to the menu."
  action: "[cancel_selection]"
```

**Alternative Styles:**

```yaml
# X style
cancel:
  material: "RED_WOOL"
  name: "<red>✖ Cancel"

# Barrier style
cancel:
  material: "BARRIER"
  name: "<red>Cancel"
```

### ⭐ Favorites GUI

Dedicated GUI for favorited message sets:

```yaml
favorite-gui:
  title: "   <st>     <reset> <dark_gray>FAVORITES <st>     "
  rows: 6
  close-on-select: false
  always-show-navigation: true
  
  favorite-message-slots:
    - "10-16"
    - "19-25"
    - "28-34"
```

#### Back Button

```yaml
items:
  back:
    slot: 49
    material: "PLAYER_HEAD"
    skull: "eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvYmViNTg4YjIxYTZmOThhZDFmZjRlMDg1YzU1MmRjYjA1MGVmYzljYWI0MjdmNDYwNDhmMThmYzgwMzQ3NWY3In19fQ=="
    name: "<reset><#FF3232>Back to Menu"
    lore:
      - "<reset><white>Click to return to the main menu."
    action: "[back]"
```

### 🎵 Sound Effects

Customize sounds for all GUI actions:

```yaml
sounds:
  open-menu:
    enabled: true
    sound: "ui.button.click"
    volume: 1.0
    pitch: 1.0

  close-menu:
    enabled: true
    sound: "ui.button.click"
    volume: 1.0
    pitch: 1.0

  select:
    enabled: true
    sound: "entity.player.levelup"
    volume: 0.5
    pitch: 1.2

  close:
    enabled: true
    sound: "block.chest.close"
    volume: 1.0
    pitch: 1.0

  back:
    enabled: true
    sound: "ui.button.click"
    volume: 1.0
    pitch: 1.0

  next_page:
    enabled: true
    sound: "item.book.page_turn"
    volume: 1.0
    pitch: 1.0

  previous_page:
    enabled: true
    sound: "item.book.page_turn"
    volume: 1.0
    pitch: 0.9

  confirm_selection:
    enabled: true
    sound: "entity.player.levelup"
    volume: 1.0
    pitch: 1.0

  cancel_selection:
    enabled: true
    sound: "block.chest.close"
    volume: 1.0
    pitch: 1.0
```

#### Sound Parameters

**Sound:** Minecraft sound name

* View all sounds: [Minecraft Sounds List](https://minecraft.fandom.com/wiki/Sounds.json)
* Format: `category.subcategory.sound`

**Volume:** 0.0 - 2.0

* `0.0` = Silent
* `1.0` = Normal volume
* `2.0` = Maximum volume

**Pitch:** 0.5 - 2.0

* `0.5` = Very low pitch
* `1.0` = Normal pitch
* `2.0` = Very high pitch

**Popular Sounds:**

```yaml
# Positive actions
sound: "entity.player.levelup"
sound: "entity.experience_orb.pickup"
sound: "block.note_block.pling"
sound: "ui.toast.challenge_complete"

# Navigation
sound: "ui.button.click"
sound: "item.book.page_turn"
sound: "block.lever.click"

# Negative actions
sound: "entity.villager.no"
sound: "block.anvil.land"
sound: "entity.enderman.teleport"

# Ambient
sound: "block.chest.open"
sound: "block.chest.close"
sound: "entity.item.pickup"
```

### 🎨 Complete GUI Examples

#### Example 1: Minimal Clean Design

```yaml
main-gui:
  title: "<white>Select Message"
  rows: 4
  close-on-select: true
  
  join-leave-message-slots:
    - "10-16"
    - "19-25"
  
  filler-item:
    material: "AIR"
    name: " "
  
  items:
    close-button:
      slot: 31
      material: "BARRIER"
      name: "<red>Close"
      action: "[close]"
```

#### Example 2: Decorative Border

```yaml
main-gui:
  title: "<gradient:#FF0000:#0000FF>Message Selection</gradient>"
  rows: 6
  
  join-leave-message-slots:
    - "10-16"
    - "19-25"
    - "28-34"
  
  items:
    # Top border
    border-top:
      slot: 0-8
      material: "GRAY_STAINED_GLASS_PANE"
      name: " "
    
    # Bottom border
    border-bottom:
      slot: 45-53
      material: "GRAY_STAINED_GLASS_PANE"
      name: " "
    
    # Side borders
    border-left:
      slot: 9,18,27,36
      material: "GRAY_STAINED_GLASS_PANE"
      name: " "
    
    border-right:
      slot: 17,26,35,44
      material: "GRAY_STAINED_GLASS_PANE"
      name: " "
    
    # Colored accents
    accent:
      slot: 46-47,51-53
      material: "PURPLE_STAINED_GLASS_PANE"
      name: " "
```

#### Example 3: Modern Design

```yaml
main-gui:
  title: "   <st>═══════<reset> <#CB1FE9><bold>BLOCKYJOIN</bold> <st>═══════   "
  rows: 6
  close-on-select: false
  
  join-leave-message-slots:
    - "10-16"
    - "19-25"
    - "28-34"
  
  filler-item:
    material: "BLACK_STAINED_GLASS_PANE"
    name: " "
  
  items:
    favorites-button:
      slot: 45
      material: "NETHER_STAR"
      name: "<#FFD700>★ Favorites"
      lore:
        - "<gray>View your favorite sets"
      action: "[open_favorites]"
    
    previous-page:
      slot: 48
      material: "ARROW"
      name: "<#49FF00>← Previous"
      action: "[previous_page]"
    
    close-button:
      slot: 49
      material: "BARRIER"
      name: "<#FF3232>✖ Close"
      action: "[close]"
    
    next-page:
      slot: 50
      material: "ARROW"
      name: "<#49FF00>Next →"
      action: "[next_page]"
```

### 💡 Design Tips

#### Layout Tips

1. **Keep it organized** - Group related items together
2. **Use borders** - Frame your content with glass panes
3. **Leave space** - Don't overcrowd the GUI
4. **Consistent theme** - Use matching colors throughout
5. **Clear navigation** - Make buttons obvious

#### Color Schemes

**Dark Theme:**

```yaml
border: "BLACK_STAINED_GLASS_PANE"
accent: "GRAY_STAINED_GLASS_PANE"
positive: "LIME_STAINED_GLASS_PANE"
negative: "RED_STAINED_GLASS_PANE"
```

**Light Theme:**

```yaml
border: "WHITE_STAINED_GLASS_PANE"
accent: "LIGHT_GRAY_STAINED_GLASS_PANE"
positive: "LIME_STAINED_GLASS_PANE"
negative: "PINK_STAINED_GLASS_PANE"
```

**Purple Theme:**

```yaml
border: "PURPLE_STAINED_GLASS_PANE"
accent: "MAGENTA_STAINED_GLASS_PANE"
positive: "PINK_STAINED_GLASS_PANE"
negative: "RED_STAINED_GLASS_PANE"
```

#### Button Placement

**Common Layouts:**

```yaml
# Bottom center
close: 49

# Bottom row spread
previous: 45
close: 49
next: 53

# Bottom corners
favorites: 45
close: 49
info: 53

# Custom bottom bar
favorites: 45
previous: 48
close: 49
next: 50
info: 53
```

### 🔧 Testing Your GUI

#### 1. Save Changes

Edit `menu.yml` and save the file.

#### 2. Reload

```
/blockyjoin reload
```

#### 3. Open GUI

```
/blockyjoin
```

#### 4. Check

* ✅ Title displays correctly
* ✅ Items appear in correct slots
* ✅ Navigation works
* ✅ Sounds play appropriately
* ✅ Colors look good

### 🐛 Troubleshooting

#### Items Not Showing

**Problem:** Items don't appear in GUI

**Solutions:**

1. Check slot numbers (0-53)
2. Verify material names
3. Check for YAML errors
4. Reload plugin

#### Wrong Slots

**Problem:** Items appear in wrong positions

**Solution:** Remember slot numbering starts at 0:

```
Slot 0 = Top-left
Slot 8 = Top-right
Slot 45 = Bottom-left
Slot 53 = Bottom-right
```

#### Title Not Formatting

**Problem:** MiniMessage tags show literally

**Solutions:**

1. Use correct MiniMessage syntax
2. Check for typos in tags
3. Verify closing tags
4. Test with simple color first

#### Sounds Not Playing

**Problem:** No sound effects

**Solutions:**

1. Check `enabled: true`
2. Verify sound name spelling
3. Test with vanilla sounds
4. Check client sound settings

### 📚 Additional Resources

* [MiniMessage Format](https://docs.advntr.dev/minimessage/format.html)
* [Minecraft Materials](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/Material.html)
* [Minecraft Sounds](https://minecraft.fandom.com/wiki/Sounds.json)
* [Player Heads Database](https://minecraft-heads.com/)

***

**See Also:**

* [Creating Message Sets](creating-message-sets.md)
* [Placeholders](placeholders.md)

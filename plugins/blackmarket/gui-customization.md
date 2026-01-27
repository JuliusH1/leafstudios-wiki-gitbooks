# 🎨 GUI Customization

Complete guide to customizing the BlackMarket GUI appearance and layout.

### Overview

The GUI is fully configurable through `config.yml`. You can customize:

* Size and layout
* Title and formatting
* Filler items
* Custom buttons and controls
* Market item display slots
* Pagination settings
* Categories

***

### GUI Size Configuration

Choose between two methods for setting GUI size:

#### Method 1: Rows (Recommended)

```yaml
gui:
  rows: 6  # 1-6 rows (automatically calculates size)
```

**Available Sizes**:

* `1` row = 9 slots
* `2` rows = 18 slots
* `3` rows = 27 slots
* `4` rows = 36 slots
* `5` rows = 45 slots
* `6` rows = 54 slots (maximum)

#### Method 2: Exact Size

```yaml
gui:
  size: 54  # Must be multiple of 9 (9-54)
```

**Valid Sizes**: `9`, `18`, `27`, `36`, `45`, `54`

***

### GUI Title

#### Basic Title

```yaml
gui:
  title: "&8&lBlack Market"
```

**Supports**:

* Legacy color codes (`&a`, `&b`, `&l`, etc.)
* MiniMessage format (if enabled)

#### MiniMessage Examples

```yaml
# Gradient title
title: "<gradient:#FFD700:#FFA500>Black Market</gradient>"

# Rainbow effect
title: "<rainbow>Black Market</rainbow>"

# Combination
title: "<gradient:#FF0000:#0000FF><bold>Black Market</bold></gradient>"
```

#### Pagination Title Format

When pagination is enabled, a page indicator is added:

```yaml
gui:
  title: "&8&lBlack Market"
  
  pagination:
    enabled: true
    title-format: " &8- &7Page %page%/%total%"
```

**Result**: `Black Market - Page 1/3`

**Available Placeholders**:

* `%page%` - Current page number
* `%total%` - Total pages

***

### Market Item Slots

Define where market items appear:

```yaml
gui:
  market-item-slots:
    - 10
    - 11
    - 12
    - 13
    - 14
    - 15
    - 16
    # ... more slots
```

#### Default Layout (6 rows)

```
[0][1][2][3][4][5][6][7][8]
[9][10][11][12][13][14][15][16][17]  ← Market items
[18][19][20][21][22][23][24][25][26] ← Market items
[27][28][29][30][31][32][33][34][35] ← Market items
[36][37][38][39][40][41][42][43][44]
[45][46][47][48][49][50][51][52][53]
```

#### Common Layouts

**Centered 3x5 Grid** (15 items):

```yaml
market-item-slots:
  - 11
  - 12
  - 13
  - 14
  - 15
  - 20
  - 21
  - 22
  - 23
  - 24
  - 29
  - 30
  - 31
  - 32
  - 33
```

**Border Layout** (22 items):

```yaml
market-item-slots:
  - 10
  - 11
  - 12
  - 13
  - 14
  - 15
  - 16
  - 19
  - 25
  - 28
  - 34
  - 37
  - 38
  - 39
  - 40
  - 41
  - 42
  - 43
```

***

### Filler Items

Fill empty slots with decorative items:

#### Basic Filler

```yaml
gui:
  filler-item:
    enabled: true
    material: "GRAY_STAINED_GLASS_PANE"
    display-name: "&7"
```

#### Custom Model Data

```yaml
filler-item:
  enabled: true
  material: "PLAYER_HEAD"
  display-name: "&7"
  custom-model-data: 12345  # For resource packs
```

#### Custom Base64 Texture

```yaml
filler-item:
  enabled: true
  material: "PLAYER_HEAD"
  display-name: "&7"
  base64-texture: "eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvYmFkYzA0OGE3Y2U3OGY3ZGFkNzJhMDdkYTI3ZDg1YzA5MTY4ODFlNTUyMmVlZWQxZTNkYWYyMTdhMzhjMWEifX19"
```

#### Specific Slots Only

```yaml
filler-item:
  enabled: true
  material: "BLACK_STAINED_GLASS_PANE"
  display-name: "&7"
  slots:  # Only fill these slots
    - 0
    - 8
    - 45
    - 53
```

**If slots not specified**: Fills all empty slots

***

### GUI Items (Buttons & Controls)

Add custom interactive items to your GUI:

#### Item Structure

```yaml
gui:
  items:
    item_key:              # Unique identifier
      enabled: true        # Toggle on/off
      material: "MATERIAL"
      display-name: "Name"
      lore: []
      custom-model-data: 0
      slot: 0              # Single slot
      # OR
      slots: [0, 1, 2]     # Multiple slots
      action: "action_name"
```

#### Available Actions

| Action           | Description              |
| ---------------- | ------------------------ |
| `close`          | Closes the GUI           |
| `next_page`      | Go to next page          |
| `previous_page`  | Go to previous page      |
| `cycle_category` | Cycle through categories |
| `none`           | No action (decorative)   |

***

### Complete GUI Item Examples

#### 1. Close Button

```yaml
close:
  enabled: true
  material: "BARRIER"
  display-name: "&c&lClose"
  lore:
    - "&7Click to close the market"
  slot: 53
  action: "close"
```

#### 2. Next Page Button

```yaml
next-page:
  enabled: true
  material: "ARROW"
  display-name: "&a&lNext Page"
  lore:
    - "&7Click to go to the next page"
    - "&7Current page: &e%page%"
  slot: 50
  action: "next_page"
```

#### 3. Previous Page Button

```yaml
previous-page:
  enabled: true
  material: "ARROW"
  display-name: "&a&lPrevious Page"
  lore:
    - "&7Click to go to the previous page"
    - "&7Current page: &e%page%"
  slot: 48
  action: "previous_page"
```

#### 4. Page Indicator

```yaml
page-info:
  enabled: true
  material: "PAPER"
  display-name: "&e&lPage %page%/%total_pages%"
  lore:
    - "&7Viewing page &e%page% &7of &e%total_pages%"
  slot: 49
  action: "none"
```

#### 5. Category Selector

```yaml
category:
  enabled: true
  material: "BOOK"
  display-name: "&6&lCategory: &e%category%"
  lore:
    - "&7Click to change category"
    - ""
    - "&eAvailable categories:"
    - "&7%categories%"
  slot: 9
  action: "cycle_category"
```

**Available Placeholders**:

* `%category%` - Current category name
* `%categories%` - List of all categories

#### 6. Reset Timer

```yaml
reset-timer:
  enabled: true
  material: "CLOCK"
  display-name: "&6&lNext Reset"
  lore:
    - "&7The market resets periodically"
    - "&7Next reset in: &e%time_until_reset%"
    - ""
    - "&8Resets every %reset_interval%"
  slot: 17
  action: "none"
```

**Available Placeholders**:

* `%time_until_reset%` - HH:MM:SS format
* `%reset_interval%` - Configured interval

#### 7. Info Display

```yaml
info:
  enabled: true
  material: "NETHER_STAR"
  display-name: "&b&lMarket Info"
  lore:
    - "&7Total items: &e%items_count%"
    - "&7Category: &e%category%"
    - "&7Type: &e%market_type%"
  slot: 0
  action: "none"
```

#### 8. Multiple Slot Item (Border)

```yaml
border:
  enabled: true
  material: "BLACK_STAINED_GLASS_PANE"
  display-name: "&7"
  slots:
    - 0
    - 1
    - 2
    - 3
    - 4
    - 5
    - 6
    - 7
    - 8
  action: "none"
```

***

### Pagination Settings

#### Enable/Disable Pagination

```yaml
gui:
  pagination:
    enabled: true  # false to disable
```

#### Title Format

```yaml
pagination:
  title-format: " &8- &7Page %page%/%total%"
```

**Examples**:

```yaml
# Simple
title-format: " - %page%/%total%"

# Fancy
title-format: " &8| &7Page &e%page% &7of &e%total%"

# Minimal
title-format: " &8(%page%/%total%)"
```

#### Items Per Page

The number of items per page is determined by `market-item-slots`:

```yaml
market-item-slots:  # 21 slots = 21 items per page
  - 10
  - 11
  # ... 19 more slots
```

***

### Category System

#### Enable Categories

```yaml
gui:
  enable-categories: true
```

#### How Categories Work

1. Items are organized by their `category` property in `items.yml`
2. Players can filter by category using the category selector button
3. "All" category shows everything

**Example categories from items.yml**:

```yaml
# items.yml
diamond_sword:
  category: "Weapons"

golden_apple:
  category: "Food"

vip_rank:
  category: "Ranks"
```

#### Category Button

```yaml
category:
  enabled: true
  material: "BOOK"
  display-name: "&6&lCategory: &e%category%"
  lore:
    - "&7Available: &e%categories%"
  slot: 9
  action: "cycle_category"
```

**Cycling Order**:

```
All → Weapons → Armor → Tools → Food → Ranks → ... → All
```

***

### Complete GUI Layouts

#### Layout 1: Classic Shop (6 rows)

```yaml
gui:
  rows: 6
  title: "&8&lBlack Market"
  
  market-item-slots:
    - 10
    - 11
    - 12
    - 13
    - 14
    - 15
    - 16
    - 19
    - 20
    - 21
    - 22
    - 23
    - 24
    - 25
    - 28
    - 29
    - 30
    - 31
    - 32
    - 33
    - 34
  
  filler-item:
    enabled: true
    material: "GRAY_STAINED_GLASS_PANE"
    display-name: "&7"
  
  items:
    info:
      enabled: true
      material: "NETHER_STAR"
      display-name: "&b&lInfo"
      slot: 0
      action: "none"
    
    category:
      enabled: true
      material: "BOOK"
      display-name: "&6&lCategory: &e%category%"
      slot: 9
      action: "cycle_category"
    
    timer:
      enabled: true
      material: "CLOCK"
      display-name: "&6&lReset: &e%time_until_reset%"
      slot: 17
      action: "none"
    
    previous:
      enabled: true
      material: "ARROW"
      display-name: "&a&lPrevious"
      slot: 48
      action: "previous_page"
    
    page-info:
      enabled: true
      material: "PAPER"
      display-name: "&e&lPage %page%/%total_pages%"
      slot: 49
      action: "none"
    
    next:
      enabled: true
      material: "ARROW"
      display-name: "&a&lNext"
      slot: 50
      action: "next_page"
    
    close:
      enabled: true
      material: "BARRIER"
      display-name: "&c&lClose"
      slot: 53
      action: "close"
```

#### Layout 2: Compact Design (4 rows)

```yaml
gui:
  rows: 4
  title: "&8Market"
  
  market-item-slots:
    - 10
    - 11
    - 12
    - 13
    - 14
    - 15
    - 16
    - 19
    - 20
    - 21
    - 22
    - 23
    - 24
    - 25
  
  filler-item:
    enabled: true
    material: "BLACK_STAINED_GLASS_PANE"
    display-name: "&7"
  
  items:
    previous:
      material: "SPECTRAL_ARROW"
      display-name: "&e← Previous"
      slot: 27
      action: "previous_page"
    
    next:
      material: "SPECTRAL_ARROW"
      display-name: "&eNext →"
      slot: 35
      action: "next_page"
    
    close:
      material: "RED_STAINED_GLASS_PANE"
      display-name: "&cClose"
      slot: 31
      action: "close"
```

#### Layout 3: Modern Borderless (5 rows)

```yaml
gui:
  rows: 5
  title: "<gradient:#FFD700:#FFA500>Market</gradient>"
  
  market-item-slots:
    - 10
    - 11
    - 12
    - 13
    - 14
    - 15
    - 16
    - 19
    - 20
    - 21
    - 22
    - 23
    - 24
    - 25
  
  filler-item:
    enabled: false  # No filler
  
  items:
    category:
      material: "COMPASS"
      display-name: "&6%category%"
      slot: 0
      action: "cycle_category"
    
    timer:
      material: "CLOCK"
      display-name: "&eReset: %time_until_reset%"
      slot: 8
      action: "none"
    
    previous:
      material: "ARROW"
      display-name: "&a◄"
      slot: 36
      action: "previous_page"
    
    next:
      material: "ARROW"
      display-name: "&a►"
      slot: 44
      action: "next_page"
```

***

### Advanced Customization

#### Custom Model Data (Resource Packs)

```yaml
gui:
  items:
    custom-button:
      material: "STONE"
      custom-model-data: 100001  # Your custom model ID
      display-name: "&eCustom Button"
      slot: 22
```

#### Player Head with Texture

```yaml
gui:
  items:
    player-head:
      material: "PLAYER_HEAD"
      display-name: "&eSpecial Head"
      base64-texture: "eyJ0ZXh0dXJlcyI6..." # Base64 texture
      slot: 4
```

Get textures from [Minecraft-Heads.com](https://minecraft-heads.com/)

***

### Sound Configuration

Configure sounds for GUI interactions:

```yaml
sounds:
  gui-open: "BLOCK_CHEST_OPEN"
  gui-close: "BLOCK_CHEST_CLOSE"
  click: "UI_BUTTON_CLICK"
  page-turn: "ITEM_BOOK_PAGE_TURN"
  purchase-success: "ENTITY_PLAYER_LEVELUP"
  purchase-fail: "ENTITY_VILLAGER_NO"
```

**Common Sounds**:

* `BLOCK_CHEST_OPEN` / `BLOCK_CHEST_CLOSE`
* `UI_BUTTON_CLICK`
* `ITEM_BOOK_PAGE_TURN`
* `ENTITY_PLAYER_LEVELUP`
* `ENTITY_VILLAGER_YES` / `ENTITY_VILLAGER_NO`
* `BLOCK_NOTE_BLOCK_PLING`

***

### Purchase Behavior

```yaml
gui:
  close-on-purchase: false  # Keep GUI open after purchase
  # true to close GUI after successful purchase
```

***

### Testing Your GUI

1. **Save config.yml**
2. **Reload**: `/bma reload config`
3. **Open GUI**: `/blackmarket`
4. **Test pagination**: Add more items than slots
5. **Test categories**: Cycle through categories
6. **Test buttons**: Click all interactive elements

***

### Common GUI Patterns

#### Pattern 1: Top Bar Navigation

```
[Info][][][Category][][][][][Timer]
[═══][Market Items...][═══]
[═══][Market Items...][═══]
[═══][Market Items...][═══]
[][Prev][][Page][][Next][][Close]
```

#### Pattern 2: Side Navigation

```
[C][Market Items...][][]
[a][Market Items...][][]
[t][Market Items...][][N]
[e][Market Items...][][e]
[g][Market Items...][][x]
[o][Market Items...][][t]
[r][Prev][Page][Next][Close]
[y]
```

#### Pattern 3: Bottom Controls

```
[Market Items...][═══]
[Market Items...][═══]
[Market Items...][═══]
[Market Items...][═══]
[═══════════════════]
[][Prev][Cat][Timer][Page][Next][][Close]
```

***

### Troubleshooting

#### Items Not Showing

1. Check `market-item-slots` list
2. Verify items exist in `items.yml`
3. Check `items-per-reset` in config
4. Enable debug: `/bma debug`

#### Buttons Not Working

1. Verify `action` is spelled correctly
2. Check button is `enabled: true`
3. Test with `/bma reload`

#### GUI Size Issues

1. Ensure size is multiple of 9
2. Check all slots are within range (0-53)
3. Verify rows/size setting

***

### Next Steps

* [Configure items](items-configuration.md)
* [Set up messages](messages-and-localization.md)
* [Learn about placeholders](placeholders.md)
* [View commands](commands-and-permissions.md)

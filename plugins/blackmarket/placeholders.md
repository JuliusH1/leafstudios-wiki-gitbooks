# 🏷️ Placeholders

BlackMarket provides extensive placeholder support for dynamic content in lore, messages, and GUI elements.

### PlaceholderAPI Integration

If PlaceholderAPI is installed, all BlackMarket placeholders are automatically available using the `%blackmarket_<placeholder>%` format.

***

### Internal Placeholders

These placeholders work in `items.yml` lore and `messages.yml` without PlaceholderAPI.

#### Item Lore Placeholders

Available in `items.yml` for item lore and display names:

| Placeholder         | Description          | Example Output |
| ------------------- | -------------------- | -------------- |
| `%price%`           | Formatted item price | `$50,000`      |
| `%player%`          | Player name          | `Steve`        |
| `%item_chance%`     | Item spawn chance    | `25`           |
| `%stock_remaining%` | Items left in stock  | `7`            |
| `%stock_max%`       | Maximum stock        | `10`           |
| `%expire_time%`     | Time until expiry    | `5h 23m`       |
| `%cooldown_time%`   | Cooldown remaining   | `2h 15m 30s`   |

**Example Usage**:

```yaml
diamond_sword:
  lore:
    - "&7Price: &e%price%"
    - "&7Stock: &e%stock_remaining%&7/&e%stock_max%"
    - "&7Rarity: &e%item_chance%%"
```

***

#### Message Placeholders

Available in `messages.yml` and `lang/` files:

| Placeholder                   | Description                | Example Output        |
| ----------------------------- | -------------------------- | --------------------- |
| `%prefix%`                    | Message prefix             | `[BlackMarket] »`     |
| `%price%`                     | Item price                 | `$50,000`             |
| `%player%`                    | Player name                | `Notch`               |
| `%time%`                      | Cooldown/time remaining    | `1h 30m`              |
| `%time_until_reset%`          | Time until next reset      | `23:45:12`            |
| `%time_until_reset_detailed%` | Detailed reset time        | `23h 45m 12s`         |
| `%reset_time%`                | Same as time\_until\_reset | `23:45:12`            |
| `%items_count%`               | Current market items       | `5`                   |
| `%market_type%`               | Market type                | `Global` / `Personal` |
| `%category%`                  | Current category           | `Weapons`             |
| `%debug_mode%`                | Debug status               | `true` / `false`      |
| `%version%`                   | Plugin version             | `2.0.4`               |
| `%database_type%`             | Database type              | `MYSQL` / `SQLITE`    |
| `%economy_provider%`          | Economy plugin             | `Vault (EssentialsX)` |
| `%server_type%`               | Server software            | `Paper` / `Folia`     |
| `%current_language%`          | Active language            | `en_us`               |

**Example Usage**:

```yaml
purchase:
  success: "%prefix% <green>You bought an item for %price%!</green>"
  
help:
  - "&6Time until reset: &e%time_until_reset_detailed%"
  - "&6Items available: &e%items_count%"
```

***

#### GUI Placeholders

Available in `config.yml` GUI item lore:

| Placeholder          | Description         | Example Output        |
| -------------------- | ------------------- | --------------------- |
| `%page%`             | Current page number | `2`                   |
| `%total_pages%`      | Total pages         | `3`                   |
| `%category%`         | Selected category   | `Weapons`             |
| `%categories%`       | All categories      | `All, Weapons, Armor` |
| `%time_until_reset%` | Time to reset       | `23:45:12`            |
| `%reset_interval%`   | Reset interval      | `24h`                 |
| `%items_count%`      | Items in market     | `5`                   |
| `%market_type%`      | Market type         | `Global`              |

**Example Usage**:

```yaml
gui:
  items:
    page-info:
      lore:
        - "&7Page &e%page%&7/&e%total_pages%"
        - "&7Category: &e%category%"
```

***

### PlaceholderAPI Placeholders

When PlaceholderAPI is installed, use these in any plugin that supports PAPI.

#### General Information

| Placeholder                               | Description                 | Example       |
| ----------------------------------------- | --------------------------- | ------------- |
| `%blackmarket_time_until_reset%`          | Time until reset (HH:MM:SS) | `23:45:12`    |
| `%blackmarket_time_until_reset_detailed%` | Detailed time               | `23h 45m 12s` |
| `%blackmarket_items_count%`               | Items in current market     | `5`           |
| `%blackmarket_market_type%`               | Global or Personal          | `Global`      |
| `%blackmarket_reset_interval%`            | Reset interval              | `24h`         |
| `%blackmarket_items_per_reset%`           | Items shown per reset       | `5`           |

***

#### Item-Specific Placeholders

| Placeholder                       | Description                | Example           |
| --------------------------------- | -------------------------- | ----------------- |
| `%blackmarket_has_bought_<item>%` | Whether player bought item | `true` / `false`  |
| `%blackmarket_cooldown_<item>%`   | Cooldown remaining         | `01:30:25`        |
| `%blackmarket_stock_<item>%`      | Stock remaining            | `7` / `Unlimited` |

**Example**: Check if player bought a specific item:

```yaml
# In another plugin's config
messages:
  vip-check: "Has VIP: %blackmarket_has_bought_vip_rank%"
  
# Scoreboard example
lines:
  - "&7Sword CD: %blackmarket_cooldown_diamond_sword%"
```

***

### Lore Block Placeholders

Special placeholders that insert entire lore blocks from `config.yml`:

| Placeholder        | Replaced With        | Configured In               |
| ------------------ | -------------------- | --------------------------- |
| `%stock%`          | Stock information    | `lore.stock.lines`          |
| `%out_of_stock%`   | Out of stock message | `lore.out-of-stock.lines`   |
| `%already_bought%` | Already purchased    | `lore.already-bought.lines` |
| `%cooldown%`       | Cooldown info        | `lore.cooldown.lines`       |
| `%expired%`        | Expired message      | `lore.expired.lines`        |
| `%expires_in%`     | Expiry countdown     | `lore.expires-in.lines`     |

**How They Work**:

When you use these in `items.yml` lore:

```yaml
diamond_sword:
  lore:
    - "&7A powerful weapon"
    - "%stock%"        # Replaced with stock lore block
    - "%cooldown%"     # Replaced with cooldown lore block
```

BlackMarket automatically inserts the appropriate lore from `config.yml`:

```yaml
# config.yml
lore:
  stock:
    enabled: true
    lines:
      - ""
      - "&7Stock: &e%stock_remaining%&7/&e%stock_max%"
  
  cooldown:
    enabled: true
    lines:
      - ""
      - "&c&lON COOLDOWN"
      - "&7Time: &e%cooldown_time%"
```

**Conditional Display**: These blocks only appear when relevant:

* `%stock%` - Only if stock is limited
* `%out_of_stock%` - Only if stock is depleted
* `%already_bought%` - Only if one-time and purchased
* `%cooldown%` - Only if on cooldown
* `%expired%` - Only if item has expired
* `%expires_in%` - Only if item has expiry time

***

### Color Code Support

All placeholders support both legacy and MiniMessage color codes:

#### Legacy Color Codes

```yaml
display-name: "&b&lDiamond Sword &7- &e%price%"
# Output: [Aqua Bold]Diamond Sword [Gray]- [Yellow]$50,000
```

#### MiniMessage Format

```yaml
display-name: "<gradient:#00FFFF:#0000FF>Ocean Sword</gradient> <gray>-</gray> <yellow>%price%</yellow>"
# Output: [Gradient]Ocean Sword [Gray]- [Yellow]$50,000
```

**Enable MiniMessage**:

```yaml
# config.yml
messages:
  use-minimessage: true
```

***

### Time Format

Time placeholders automatically format durations:

| Input Milliseconds   | Short Format | Detailed Format |
| -------------------- | ------------ | --------------- |
| `3661000` (1h 1m 1s) | `01:01:01`   | `1h 1m 1s`      |
| `86400000` (1 day)   | `24:00:00`   | `1d`            |
| `7200000` (2 hours)  | `02:00:00`   | `2h`            |
| `60000` (1 minute)   | `01:00`      | `1m`            |

**Available Formats**:

* `%time_until_reset%` → `23:45:12`
* `%time_until_reset_detailed%` → `23h 45m 12s`
* `%expire_time%` → `5h 23m`
* `%cooldown_time%` → `2h 15m 30s`

***

### External Plugin Integration

#### Scoreboard (via DeluxeScoreboard, FeatherBoard, etc.)

```yaml
# DeluxeScoreboard
lines:
  - "&6&lBlackMarket"
  - ""
  - "&7Items: &e%blackmarket_items_count%"
  - "&7Reset: &e%blackmarket_time_until_reset%"
  - ""
  - "&7Type: &e%blackmarket_market_type%"
```

#### Tab List (via TAB, NametagEdit, etc.)

```yaml
# TAB plugin
header:
  - "&6&lServer Shop"
  - "&7Next reset: &e%blackmarket_time_until_reset_detailed%"
```

#### Chat (via ChatControl, EssentialsX Chat, etc.)

```yaml
# ChatControl format
format: "&7[%blackmarket_market_type%] &r{player}: {message}"
```

#### Holograms (via DecentHolograms, HolographicDisplays, etc.)

```yaml
# DecentHolograms
lines:
  - "&6&lBlackMarket Shop"
  - "&7Items available: &e%blackmarket_items_count%"
  - "&7Reset in: &e%blackmarket_time_until_reset%"
```

#### NPCs (via Citizens, ZNPCs, etc.)

```yaml
# Citizens
text:
  - "Welcome to the Black Market!"
  - "Items available: %blackmarket_items_count%"
  - "Next reset: %blackmarket_time_until_reset%"
```

***

### Creating Dynamic Messages

#### Example 1: Purchase Notification

```yaml
# messages.yml
purchase:
  success: "%prefix% <green>You purchased an item for <yellow>%price%</yellow>!</green>"
  insufficient-funds: "%prefix% <red>Need <yellow>%price%</yellow> but only have <yellow>%balance%</yellow>!</red>"
```

#### Example 2: Market Info

```yaml
info-player:
  - ""
  - "<gold>BlackMarket Info</gold>"
  - "<gray>━━━━━━━━━━━━━━━━━</gray>"
  - "<gold>Type:</gold> <yellow>%market_type%</yellow>"
  - "<gold>Items:</gold> <yellow>%items_count%</yellow>"
  - "<gold>Reset:</gold> <yellow>%time_until_reset_detailed%</yellow>"
  - "<gold>Category:</gold> <yellow>%category%</yellow>"
  - "<gray>━━━━━━━━━━━━━━━━━</gray>"
```

#### Example 3: Item Lore

```yaml
# items.yml
legendary_item:
  lore:
    - "<gradient:#FF0000:#FFD700>LEGENDARY ITEM</gradient>"
    - ""
    - "<gray>Only <yellow>%stock_remaining%</yellow> remaining!</gray>"
    - "<gray>Rarity: <gold>%item_chance%%</gold></gray>"
    - ""
    - "%expires_in%"
    - "%cooldown%"
    - ""
    - "<yellow>Price: <gold>%price%</gold></yellow>"
```

***

### Custom Placeholder Integration

You can use PlaceholderAPI expansions alongside BlackMarket placeholders:

```yaml
# Combining multiple expansions
lore:
  - "&7Player: &e%player_name%"                    # PAPI
  - "&7Balance: &e%vault_eco_balance_formatted%"   # Vault expansion
  - "&7Market reset: &e%blackmarket_time_until_reset%" # BlackMarket
  - "&7Server: &e%server_name%"                    # PAPI
```

***

### Placeholder Conditions

Some placeholders are conditional:

| Placeholder        | Shown When                         |
| ------------------ | ---------------------------------- |
| `%stock%`          | Item has stock limit (`stock: 10`) |
| `%out_of_stock%`   | Stock reaches 0                    |
| `%already_bought%` | Item is one-time and purchased     |
| `%cooldown%`       | Player is on cooldown              |
| `%expired%`        | Item expiry time passed            |
| `%expires_in%`     | Item has expiry configured         |

***

### Debugging Placeholders

If placeholders aren't working:

1. **Check spelling**: Placeholders are case-sensitive
2. **Verify PlaceholderAPI**: `/papi parse me %blackmarket_items_count%`
3. **Enable debug**: `/bma debug`
4. **Check logs**: Look for "placeholder" errors
5. **Test in-game**: Use `/bma info` to see current values

***

### Next Steps

* [Configure messages](messages-and-localization.md)
* [Customize GUI](gui-customization.md)
* [Set up items](items-configuration.md)
* [Learn about commands](commands-and-permissions.md)

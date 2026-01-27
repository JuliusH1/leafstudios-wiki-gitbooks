# 📦 Items Configuration

The `items.yml` file defines all items available in your BlackMarket. Each item is highly configurable with prices, economies, stock limits, cooldowns, and more.

### File Location

```
plugins/BlackMarket/items.yml
```

### Basic Item Structure

```yaml
items:
  item_key:                    # Unique identifier
    material: "DIAMOND"        # Required: Minecraft material
    display-name: "&bDiamond"  # Required: Item name
    lore:                      # Optional: Item description
      - "&7A shiny diamond"
    price: 1000                # Required: Item cost
```

### Complete Property Reference

#### Core Properties

**`material` (Required)**

The Minecraft material type for the item.

```yaml
material: "DIAMOND_SWORD"
```

**Valid Values**: Any valid Minecraft material name (e.g., `DIAMOND`, `NETHERITE_SWORD`, `GOLDEN_APPLE`)

***

**`display-name` (Required)**

The name shown in the GUI. Supports color codes and MiniMessage.

```yaml
# Legacy color codes
display-name: "&b&lShiny Sword"

# MiniMessage format
display-name: "<gradient:#FF0000:#0000FF>Rainbow Sword</gradient>"
```

***

**`lore` (Optional)**

Description lines shown below the item name.

```yaml
lore:
  - "&7A powerful weapon"
  - "&7with special effects"
  - ""
  - "&6Price: &e%price%"
  - "&eClick to purchase!"
```

**Available Placeholders in Lore**:

* `%price%` - Formatted price
* `%player%` - Player name
* `%item_chance%` - Spawn chance percentage
* `%stock_remaining%` - Items left in stock
* `%stock_max%` - Maximum stock
* `%expire_time%` - Time until expiry
* `%cooldown_time%` - Cooldown remaining

**Special Lore Blocks**: These placeholders are replaced with configured lore blocks:

* `%stock%` - Stock information
* `%out_of_stock%` - Out of stock message
* `%already_bought%` - Already purchased message
* `%cooldown%` - Cooldown information
* `%expired%` - Expired message
* `%expires_in%` - Expiry countdown

***

**`price` (Required)**

The cost of the item in the specified economy.

```yaml
price: 50000
```

**Supports decimals**: `price: 99.99`

***

#### Economy Settings

**`economy-type` (Optional)**

Which economy system to use for this item.

```yaml
economy-type: "vault"
```

**Valid Values**:

* `vault` - Vault economy (default)
* `playerpoints` - PlayerPoints
* `coinsengine` - CoinsEngine default currency
* `coinsengine_<currency_id>` - Specific CoinsEngine currency
* `exp` / `experience` / `levels` / `xp` - Experience levels

**Examples**:

```yaml
# Use Vault economy
economy-type: "vault"
price: 10000

# Use PlayerPoints
economy-type: "playerpoints"
price: 500

# Use experience levels
economy-type: "exp"
price: 30

# Use specific CoinsEngine currency
economy-type: "coinsengine_gems"
price: 100
```

***

#### Purchase Behavior

**`command` (Optional)**

Command executed when item is purchased. Use `%player%` for player name.

```yaml
command: "give %player% diamond_sword 1"
```

**Multiple Commands**:

```yaml
command: "give %player% diamond_sword 1; lp user %player% permission set special.perk true"
```

***

**`message` (Optional)**

Custom message sent to player on successful purchase.

```yaml
message: "&aYou purchased a &b&lShiny Sword&a!"
```

If not specified, uses default `purchase.success` message.

***

**`permission` (Optional)**

Permission required to purchase this item.

```yaml
permission: "blackmarket.buy.legendary"
```

Players without this permission will see `purchase.no-permission` message.

***

#### Stock & Availability

**`stock` (Optional)**

Maximum number of times this item can be sold.

```yaml
stock: 10   # Can be sold 10 times total
stock: -1   # Unlimited stock (default)
```

**Behavior**:

* Stock is tracked globally (all players share the pool)
* When stock reaches 0, item shows as "OUT OF STOCK"
* Stock resets when market rotates (unless using Redis with persistence)

***

**`one-time` (Optional)**

Whether a player can only buy this item once.

```yaml
one-time: true  # Player can only buy once ever
one-time: false # Player can buy multiple times (default)
```

**Use Cases**:

* Rank upgrades
* One-time kits
* Special achievements

***

**`cooldown` (Optional)**

Time before a player can purchase this item again.

```yaml
cooldown: "1h"    # 1 hour
cooldown: "30m"   # 30 minutes
cooldown: "24h"   # 24 hours
cooldown: "7d"    # 7 days
```

**Time Format**:

* `s` - seconds
* `m` - minutes
* `h` - hours
* `d` - days

**Examples**: `30s`, `5m`, `2h`, `1d`

***

**`expiry` (Optional)**

How long after market rotation this item remains available.

```yaml
expiry: "12h"  # Available for 12 hours after appearing
```

**Use Cases**:

* Limited-time offers
* Flash sales
* Early-bird specials

**Note**: Item expires relative to the last market reset time.

***

#### Market Appearance

**`category` (Optional)**

Category for filtering in the GUI.

```yaml
category: "Weapons"
```

**Common Categories**:

* Weapons
* Armor
* Tools
* Food
* Ranks
* Boosters
* Cosmetics
* Misc

Players can filter by category if `enable-categories: true` in config.

***

**`chance` (Optional)**

Probability (0-100) that this item appears in market rotation.

```yaml
chance: 100  # Always appears (default)
chance: 50   # 50% chance to appear
chance: 10   # 10% chance (rare)
chance: 1    # 1% chance (ultra rare)
```

**Rarity System**:

* `100` - Common (always appears)
* `75-99` - Uncommon
* `50-74` - Rare
* `25-49` - Very Rare
* `1-24` - Ultra Rare

Use `%item_chance%` in lore to display rarity.

***

#### Visual Customization

**`custom-model-data` (Optional)**

Custom model data for resource packs.

```yaml
custom-model-data: 12345
```

Allows custom textures/models via resource packs.

***

**`sound` (Optional)**

Sound played when item is purchased.

```yaml
sound: "ENTITY_PLAYER_LEVELUP"
```

**Common Sounds**:

* `ENTITY_PLAYER_LEVELUP` - Success sound
* `ENTITY_VILLAGER_YES` - Positive sound
* `BLOCK_NOTE_BLOCK_PLING` - Pling sound
* `UI_TOAST_CHALLENGE_COMPLETE` - Achievement sound

If not specified, uses `sounds.purchase-success` from config.

***

**`effect` (Optional)**

Particle effect spawned when purchased.

```yaml
effect: "VILLAGER_HAPPY"
```

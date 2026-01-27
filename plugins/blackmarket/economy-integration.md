# 💰 Economy Integration

Complete guide to integrating BlackMarket with various economy plugins.

### Supported Economy Systems

BlackMarket supports four economy types:

1. **Vault** - Universal economy API (most common)
2. **PlayerPoints** - Points-based system
3. **CoinsEngine** - Multi-currency plugin
4. **Experience Levels** - Minecraft's built-in XP system

***

### Vault Integration

#### Overview

Vault is the most common economy system, working as a bridge between BlackMarket and other economy plugins.

#### Setup

**Step 1**: Install Vault

```
Download from: https://www.spigotmc.org/resources/vault.34315/
Place in: plugins/Vault.jar
```

**Step 2**: Install an economy plugin

Choose one:

* **EssentialsX** (most popular)
* **CMI**
* **CraftConomy**
* **AConomy**
* **GrandEconomy**

**Step 3**: Configure BlackMarket

```yaml
# config.yml
economy:
  default: "vault"
```

**Step 4**: Restart server

#### Verification

Check if Vault is detected:

```
/bma info

# Should show:
# Economy: Vault (EssentialsX)
```

#### Item Configuration

Items use Vault by default:

```yaml
# items.yml
diamond_sword:
  material: "DIAMOND_SWORD"
  display-name: "&bDiamond Sword"
  price: 10000
  economy-type: "vault"  # Optional, this is default
```

#### Common Vault Economy Plugins

**EssentialsX**

```yaml
# Install: EssentialsX + Vault
# Auto-detected by BlackMarket
# Uses /balance, /pay, /eco commands
```

**Starting Balance**:

```yaml
# EssentialsX/config.yml
starting-balance: 1000
```

**CMI**

```yaml
# Install: CMI + Vault
# Auto-detected by BlackMarket
# Uses CMI's economy system
```

**CraftConomy**

```yaml
# Install: CraftConomy3 + Vault
# Supports multiple currencies
# BlackMarket uses default currency
```

***

### PlayerPoints Integration

#### Overview

PlayerPoints is a separate points system independent of money.

#### Setup

**Step 1**: Install PlayerPoints

```
Download from: https://www.spigotmc.org/resources/playerpoints.80745/
Place in: plugins/PlayerPoints.jar
```

**Step 2**: Restart server

**Step 3**: Verify detection

```
/bma info

# Should show:
# Economy: PlayerPoints
```

#### Item Configuration

```yaml
# items.yml
vip_rank:
  material: "NETHER_STAR"
  display-name: "&5VIP Rank"
  price: 500            # 500 points
  economy-type: "playerpoints"
  command: "lp user %player% parent add vip"
```

#### PlayerPoints Commands

```
/points          # Check your points
/points pay <player> <amount>
/points give <player> <amount>   # Admin
/points take <player> <amount>   # Admin
/points set <player> <amount>    # Admin
```

#### Use Cases

* **Vote rewards**: Give points for voting
* **Minigame rewards**: Award points for winning
* **Daily bonuses**: Points instead of money
* **Separate economy**: Keep server economy and shop separate

***

### CoinsEngine Integration

#### Overview

CoinsEngine supports multiple currencies simultaneously. BlackMarket can use any configured currency.

#### Setup

**Step 1**: Install CoinsEngine

```
Download from: https://www.spigotmc.org/resources/coinsengine.84121/
Place in: plugins/CoinsEngine.jar
```

**Step 2**: Configure currencies

```yaml
# CoinsEngine/currencies/coins.yml
coins:
  name: "Coins"
  symbol: "⛃"
  # ... other settings
```

**Step 3**: Restart server

**Step 4**: Verify detection

```
/bma info

# Should show:
# Economy: CoinsEngine
```

#### Currency Configuration

**Default Currency**

```yaml
# items.yml
diamond_sword:
  price: 100
  economy-type: "coinsengine"  # Uses first/default currency
```

**Specific Currency**

```yaml
# items.yml
# Use specific currency by ID
premium_item:
  price: 50
  economy-type: "coinsengine_gems"  # Uses "gems" currency
```

**Currency ID format**: `coinsengine_<currency_id>`

#### Example: Multiple Currencies

```yaml
# CoinsEngine has: coins, gems, tokens

# Item 1: Costs Coins
sword:
  price: 1000
  economy-type: "coinsengine_coins"

# Item 2: Costs Gems
armor:
  price: 50
  economy-type: "coinsengine_gems"

# Item 3: Costs Tokens
rank:
  price: 10
  economy-type: "coinsengine_tokens"
```

#### CoinsEngine Commands

```
/coins balance               # Check balance
/coins balance <currency>    # Check specific currency
/coins pay <player> <amount> <currency>
/coins give <player> <amount> <currency>  # Admin
/coins take <player> <amount> <currency>  # Admin
```

#### Troubleshooting CoinsEngine

If CoinsEngine isn't detected:

1. **Check version**: Use CoinsEngine 2.0+
2. **Check logs**: Look for hook messages
3. **Enable debug**: `/bma debug`
4. **List currencies**:

```
/bma info
# Shows available currencies in logs
```

***

### Experience Levels Integration

#### Overview

Use Minecraft's built-in experience system as currency. No plugin required!

#### Setup

**Already built-in** - No installation needed!

#### Item Configuration

```yaml
# items.yml
xp_bottle:
  material: "EXPERIENCE_BOTTLE"
  display-name: "&aXP Bottle"
  price: 30              # 30 levels
  economy-type: "exp"    # or "experience", "levels", "xp"
  command: "give %player% experience_bottle 16"
```

**All these work**:

```yaml
economy-type: "exp"
economy-type: "experience"
economy-type: "levels"
economy-type: "xp"
```

#### Use Cases

* **Early game shops**: Before economy setup
* **Adventure servers**: Trade XP for items
* **Skyblock**: Alternative to money
* **Survival**: Simple economy without plugins

#### Limitations

* ⚠️ Only whole levels (no decimals)
* ⚠️ Limited to player's level
* ⚠️ XP orbs don't count, only levels

***

### Mixed Economy Setup

#### Using Multiple Economies

You can use different economy types for different items:

```yaml
# items.yml

# Money (Vault)
expensive_rank:
  price: 50000
  economy-type: "vault"

# Points (PlayerPoints)
cosmetic_item:
  price: 1000
  economy-type: "playerpoints"

# Gems (CoinsEngine)
premium_kit:
  price: 100
  economy-type: "coinsengine_gems"

# XP (Experience)
enchanted_book:
  price: 30
  economy-type: "exp"
```

#### Example: Tiered System

```yaml
# Tier 1: XP (starter items)
stone_sword:
  price: 5
  economy-type: "exp"

# Tier 2: Points (mid-tier)
iron_sword:
  price: 100
  economy-type: "playerpoints"

# Tier 3: Money (high-tier)
diamond_sword:
  price: 10000
  economy-type: "vault"

# Tier 4: Premium Currency (special)
legendary_sword:
  price: 50
  economy-type: "coinsengine_gems"
```

***

### Default Economy

#### Setting Default

```yaml
# config.yml
economy:
  default: "vault"  # Used when economy-type not specified
```

**Available defaults**:

* `vault`
* `playerpoints`
* `coinsengine`
* `exp` (or `experience`, `levels`, `xp`)

#### Fallback Behavior

If specified economy not available:

1. Tries default economy
2. Shows error to player
3. Logs warning to console

***

### Economy Providers Comparison

| Feature            | Vault              | PlayerPoints | CoinsEngine | Experience |
| ------------------ | ------------------ | ------------ | ----------- | ---------- |
| **Setup**          | Medium             | Easy         | Medium      | None       |
| **Multi-Currency** | ❌                  | ❌            | ✅           | ❌          |
| **Decimal Values** | ✅                  | ❌            | ✅           | ❌          |
| **Commands**       | Via economy plugin | Built-in     | Built-in    | Built-in   |
| **Database**       | Via economy plugin | Built-in     | Built-in    | N/A        |
| **Cross-Server**   | Depends            | ✅            | ✅           | ❌          |

***

### Economy Commands Reference

#### Vault (EssentialsX Example)

```bash
/balance             # Check balance
/balance <player>    # Check other's balance
/pay <player> <amount>
/eco give <player> <amount>   # Admin
/eco take <player> <amount>   # Admin
/eco set <player> <amount>    # Admin
```

#### PlayerPoints

```bash
/points              # Check points
/points pay <player> <amount>
/points give <player> <amount>  # Admin
/points take <player> <amount>  # Admin
```

#### CoinsEngine

```bash
/coins balance
/coins balance <currency>
/coins pay <player> <amount> <currency>
/coins give <player> <amount> <currency>  # Admin
```

#### Experience

```bash
/xp                  # Check XP
/xp set <player> <amount>L    # Set levels (admin)
/xp add <player> <amount>L    # Add levels (admin)
```

***

### Troubleshooting

#### Issue 1: No Economy Found

**Error**: `No economy plugin found!`

**Solutions**:

1. **Install economy plugin**:
   * Vault + EssentialsX
   * OR PlayerPoints
   * OR CoinsEngine
2. **Check plugin loaded**:

```
/plugins
# Should see: Vault, PlayerPoints, or CoinsEngine
```

3. **Verify detection**:

```
/bma info
# Should show economy provider
```

#### Issue 2: Wrong Economy Used

**Problem**: Item charges wrong currency

**Solution**: Specify economy type

```yaml
my_item:
  price: 1000
  economy-type: "vault"  # Explicitly set
```

#### Issue 3: Decimal Points Not Working

**Problem**: PlayerPoints or XP shows decimals

**Solution**: These only support whole numbers

```yaml
# Wrong
price: 99.99
economy-type: "playerpoints"

# Correct
price: 100
economy-type: "playerpoints"
```

#### Issue 4: Multiple Vaults Detected

**Problem**: Multiple economy plugins cause conflicts

**Solution**:

1. Choose one economy plugin
2. Disable others
3. Restart server

***

### Best Practices

#### 1. Consistent Economy

Use one main economy for most items:

```yaml
# Good - Mostly Vault
economy:
  default: "vault"

# 90% of items use Vault (default)
# 10% use special currencies
```

#### 2. Clear Labels

Show economy type in lore:

```yaml
diamond_sword:
  lore:
    - "&7Price: &e$10,000 &7(Money)"

vip_rank:
  lore:
    - "&7Price: &e500 Points"

gem_item:
  lore:
    - "&7Price: &e50 Gems"
```

#### 3. Balanced Pricing

Consider conversion rates:

```
$1,000 Vault = 100 Points = 10 Gems = 5 Levels
```

#### 4. Economy Icons

Use different materials for clarity:

```yaml
# Money items
money_item:
  material: "GOLD_INGOT"
  economy-type: "vault"

# Points items
points_item:
  material: "EMERALD"
  economy-type: "playerpoints"

# XP items
xp_item:
  material: "EXPERIENCE_BOTTLE"
  economy-type: "exp"
```

***

### Advanced Configurations

#### Economy Permission Gates

```yaml
legendary_item:
  price: 100000
  economy-type: "vault"
  permission: "blackmarket.buy.legendary"
```

Only players with permission can see/buy.

#### Dynamic Pricing (via Commands)

Use command to adjust price:

```yaml
dynamic_item:
  price: 5000
  command: |
    execute if score @p money matches 10000.. run eco take %player% 5000
    execute unless score @p money matches 10000.. run eco take %player% 10000
```

#### Multi-Step Purchases

```yaml
combo_pack:
  price: 1000
  economy-type: "vault"
  command: |
    give %player% diamond 10;
    points give %player% 100;
    xp add %player% 5L
```

Deducts money, gives items, points, and XP.

***

### Next Steps

* [Configure items with economies](items-configuration.md#economy-settings)
* [Set up database](database.md)
* [Learn about troubleshooting](troubleshooting-guide.md)
* [View FAQ](frequently-asked-questions.md)

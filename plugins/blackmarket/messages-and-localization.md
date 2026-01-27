# 🌐 Messages & Localization

Complete guide to customizing messages and adding multi-language support.

### Overview

BlackMarket supports two message systems:

1. **Legacy Mode**: Single `messages.yml` file
2. **Localization Mode**: Multiple language files in `lang/` folder

***

### Configuration

#### Enable/Disable Localization

```yaml
# config.yml
messages:
  # Use MiniMessage for modern formatting
  use-minimessage: true
  
  # Enable localization system
  localization: false  # true = use lang/ files
  
  # Active language (only if localization enabled)
  lang: "EN_US"  # Filename without .yml
```

***

### Message Formatting

#### MiniMessage Format (Recommended)

When `use-minimessage: true`:

```yaml
# Simple colors
message: "<red>Error!</red>"
message: "<green>Success!</green>"

# Gradients
message: "<gradient:#FFD700:#FFA500>BlackMarket</gradient>"

# Multiple effects
message: "<bold><gradient:#FF0000:#0000FF>Title</gradient></bold>"

# Rainbow
message: "<rainbow>Colorful Text</rainbow>"

# Hover and click events
message: "<hover:show_text:'Click me!'><click:run_command:/shop>Open Shop</click></hover>"
```

**Common Tags**:

* Colors: `<red>`, `<blue>`, `<green>`, `<yellow>`, `<gold>`, `<aqua>`, `<white>`, `<gray>`, `<dark_gray>`, `<black>`
* Formatting: `<bold>`, `<italic>`, `<underlined>`, `<strikethrough>`, `<obfuscated>`
* Special: `<gradient:#START:#END>`, `<rainbow>`, `<reset>`

#### Legacy Color Codes

When `use-minimessage: false`:

```yaml
# Colors
message: "&cError!"
message: "&aSuccess!"

# Formatting
message: "&l&bBold Blue"
message: "&o&eItalic Yellow"

# Combined
message: "&6&lBlack&e&lMarket"
```

**Color Codes**:

* `&0-9, a-f` - Colors
* `&l` - Bold
* `&o` - Italic
* `&n` - Underline
* `&m` - Strikethrough
* `&k` - Obfuscated
* `&r` - Reset

**Note**: Both formats work together when MiniMessage is enabled!

***

### Legacy Mode (messages.yml)

#### File Location

```
plugins/BlackMarket/messages.yml
```

#### Structure

```yaml
# Prefix used in all messages
prefix: "<gradient:#FFD700:#FFA500><bold>BlackMarket</bold></gradient> <dark_gray>»</dark_gray>"

# General messages
no-permission: "%prefix% <red>You don't have permission.</red>"
player-only: "%prefix% <red>Only players can use this.</red>"

# Purchase messages
purchase:
  success: "%prefix% <green>Purchase successful!</green>"
  failed: "%prefix% <red>Transaction failed.</red>"

# Lists
help:
  - ""
  - "<gold>Help Menu</gold>"
  - "<gray>Command 1"
  - "<gray>Command 2"
  - ""
```

#### Using the Prefix

The `%prefix%` placeholder automatically includes your configured prefix:

```yaml
prefix: "[Shop] »"

# This message:
no-permission: "%prefix% <red>No permission!</red>"

# Displays as:
# [Shop] » No permission!
```

***

### Localization Mode (lang/ files)

#### Enable Localization

```yaml
# config.yml
messages:
  localization: true
  lang: "EN_US"  # Without .yml extension
```

#### File Location

```
plugins/BlackMarket/lang/EN_US.yml
plugins/BlackMarket/lang/ES_ES.yml
plugins/BlackMarket/lang/FR_FR.yml
```

#### Creating Language Files

1. **Copy default language**:

```bash
cp plugins/BlackMarket/lang/EN_US.yml plugins/BlackMarket/lang/DE_DE.yml
```

2. **Translate messages**:

```yaml
# DE_DE.yml (German)
prefix: "<gradient:#FFD700:#FFA500><bold>SchwarzMarkt</bold></gradient> <dark_gray>»</dark_gray>"

no-permission: "%prefix% <red>Du hast keine Berechtigung.</red>"

purchase:
  success: "%prefix% <green>Kauf erfolgreich!</green>"
  insufficient-funds: "%prefix% <red>Nicht genug Geld! Benötigt: <yellow>%price%</yellow></red>"
```

3. **Switch language**:

```yaml
# config.yml
messages:
  lang: "DE_DE"
```

4. **Reload**:

```
/bma reload messages
```

***

### Complete Message Reference

#### General Messages

```yaml
prefix: "<gradient:#FFD700:#FFA500><bold>BlackMarket</bold></gradient> <dark_gray>»</dark_gray>"

no-permission: "%prefix% <red>You don't have permission to do this.</red>"
player-only: "%prefix% <red>Only players can use this command.</red>"
reload-success: "%prefix% <green>Configuration reloaded successfully!</green>"
market-reset: "%prefix% <yellow>The market has been reset with new items!</yellow>"
```

#### Purchase Messages

```yaml
purchase:
  success: "%prefix% <green>Purchase successful!</green>"
  failed: "%prefix% <red>Transaction failed. Please try again.</red>"
  insufficient-funds: "%prefix% <red>You don't have enough money! Required: <yellow>%price%</yellow></red>"
  out-of-stock: "%prefix% <red>This item is out of stock!</red>"
  already-bought: "%prefix% <red>You have already purchased this item!</red>"
  on-cooldown: "%prefix% <red>This item is on cooldown! Time remaining: <yellow>%time%</yellow></red>"
  expired: "%prefix% <red>This item has expired!</red>"
  no-permission: "%prefix% <red>You don't have permission to buy this item!</red>"
```

**Available Placeholders**:

* `%price%` - Item price
* `%time%` - Cooldown remaining
* `%player%` - Player name

#### Admin Messages

```yaml
admin:
  reset: "%prefix% <yellow>Market has been forcefully reset!</yellow>"
  player-not-found: "%prefix% <red>Player not found!</red>"
  opened-for-player: "%prefix% <green>Opened market for <yellow>%player%</yellow>.</green>"
  usage: "%prefix% <gray>Usage: /blackmarketadmin <subcommand></gray>"
  usage-open: "%prefix% <red>Usage: /blackmarketadmin open <player></red>"
  unknown-command: "%prefix% <red>Unknown command! Use /bma help for help.</red>"
  debug-toggled: "%prefix% <yellow>Debug mode has been toggled to <gold>%debug_mode%</gold>!</yellow>"
```

#### Help Messages

```yaml
help:
  - ""
  - "<gradient:#FFD700:#FFA500><bold>BlackMarket Help</bold></gradient>"
  - "<gray>━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</gray>"
  - "<yellow>/blackmarket</yellow> <gray>- Open the market"
  - "<yellow>/blackmarket help</yellow> <gray>- Show this help"
  - "<yellow>/blackmarket info</yellow> <gray>- Show market info"
  - ""
  - "<gold>Next Reset:</gold> <yellow>%time_until_reset_detailed%</yellow>"
  - "<gray>━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</gray>"
  - ""

admin-help:
  - ""
  - "<gradient:#FFD700:#FFA500><bold>BlackMarket Admin Commands</bold></gradient>"
  - "<gray>━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</gray>"
  - "<yellow>/bma help</yellow> <gray>- Show all commands"
  - "<yellow>/bma reload</yellow> <gray>- Reload configuration"
  - "<yellow>/bma reset</yellow> <gray>- Force market reset"
  - "<yellow>/bma open <player></yellow> <gray>- Open market for player"
  - "<yellow>/bma info</yellow> <gray>- Show plugin info"
  - "<yellow>/bma debug</yellow> <gray>- Toggle debug mode"
  - "<gray>━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</gray>"
  - ""
```

#### Info Messages

```yaml
info-player:
  - ""
  - "<gradient:#FFD700:#FFA500><bold>BlackMarket Info</bold></gradient>"
  - "<gray>━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</gray>"
  - "<gold>Market Type:</gold> <yellow>%market_type%</yellow>"
  - "<gold>Available Items:</gold> <yellow>%items_count%</yellow>"
  - "<gold>Next Reset:</gold> <yellow>%time_until_reset_detailed%</yellow>"
  - "<gray>━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</gray>"
  - ""

admin-info:
  - ""
  - "<gradient:#FFD700:#FFA500><bold>BlackMarket Admin Info</bold></gradient>"
  - "<gray>━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</gray>"
  - "<gold>Version:</gold> <yellow>%version%</yellow>"
  - "<gold>Server:</gold> <yellow>%server_type%</yellow>"
  - "<gold>Market Type:</gold> <yellow>%market_type%</yellow>"
  - "<gold>Database:</gold> <yellow>%database_type%</yellow>"
  - "<gold>Economy:</gold> <yellow>%economy_provider%</yellow>"
  - "<gold>Online Players:</gold> <yellow>%online_players%</yellow>"
  - "<gold>Language:</gold> <yellow>%current_language%</yellow>"
  - "<gray>━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</gray>"
  - ""

debug-info:
  - ""
  - "<gradient:#FFD700:#FFA500><bold>BlackMarket Debug Info</bold></gradient>"
  - "<gray>━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</gray>"
  - "<gold>Debug Mode:</gold> <yellow>%debug_mode%</yellow>"
  - "<gold>Folia Support:</gold> <yellow>%folia_enabled%</yellow>"
  - "<gold>Reset Interval:</gold> <yellow>%reset_interval%</yellow>"
  - "<gold>Items Per Reset:</gold> <yellow>%items_per_reset%</yellow>"
  - "<gold>Pagination:</gold> <yellow>%pagination_enabled%</yellow>"
  - "<gold>Categories:</gold> <yellow>%categories_enabled%</yellow>"
  - "<gold>MiniMessage:</gold> <yellow>%minimessage_enabled%</yellow>"
  - "<gold>Localization:</gold> <yellow>%localization_enabled%</yellow>"
  - "<gold>Current Language:</gold> <yellow>%current_language%</yellow>"
  - "<gray>━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</gray>"
  - ""
```

#### Error Messages

```yaml
error:
  database-connection: "%prefix% <red>Failed to connect to database!</red>"
  economy-missing: "%prefix% <red>No economy plugin found!</red>"
  config-error: "%prefix% <red>Error in configuration: %error%</red>"
  command-error: "%prefix% <red>An error occurred while executing this command.</red>"
```

#### Action Bar Messages

```yaml
actionbar:
  purchase-success: "<green>✓</green> <gray>Purchase successful!</gray>"
  purchase-failed: "<red>✘</red> <gray>Purchase failed!</gray>"
  insufficient-funds: "<red>✘</red> <gray>Not enough money!</gray>"
  page-changed: "<yellow>→</yellow> <gray>Page %page%/%total_pages%</gray>"
  category-changed: "<yellow>→</yellow> <gray>Category: %category%</gray>"
```

#### GUI Messages

```yaml
gui:
  loading: "<gray>Loading market...</gray>"
  no-items: "<red>No items available!</red>"
  market-closed: "<red>The market is currently closed!</red>"
```

***

### Creating Custom Languages

#### Example: Spanish (ES\_ES.yml)

```yaml
# Spanish Translation
prefix: "<gradient:#FFD700:#FFA500><bold>MercadoNegro</bold></gradient> <dark_gray>»</dark_gray>"

no-permission: "%prefix% <red>No tienes permiso para hacer esto.</red>"
player-only: "%prefix% <red>Solo los jugadores pueden usar este comando.</red>"
reload-success: "%prefix% <green>¡Configuración recargada exitosamente!</green>"
market-reset: "%prefix% <yellow>¡El mercado ha sido reiniciado con nuevos artículos!</yellow>"

purchase:
  success: "%prefix% <green>¡Compra exitosa!</green>"
  failed: "%prefix% <red>Transacción fallida. Por favor intenta de nuevo.</red>"
  insufficient-funds: "%prefix% <red>¡No tienes suficiente dinero! Requerido: <yellow>%price%</yellow></red>"
  out-of-stock: "%prefix% <red>¡Este artículo está agotado!</red>"
  already-bought: "%prefix% <red>¡Ya has comprado este artículo!</red>"
  on-cooldown: "%prefix% <red>¡Este artículo está en tiempo de espera! Tiempo restante: <yellow>%time%</yellow></red>"
  expired: "%prefix% <red>¡Este artículo ha expirado!</red>"

help:
  - ""
  - "<gradient:#FFD700:#FFA500><bold>Ayuda de MercadoNegro</bold></gradient>"
  - "<gray>━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</gray>"
  - "<yellow>/mercadonegro</yellow> <gray>- Abrir el mercado"
  - "<yellow>/mercadonegro ayuda</yellow> <gray>- Mostrar esta ayuda"
  - "<yellow>/mercadonegro info</yellow> <gray>- Mostrar información del mercado"
  - ""
  - "<gold>Próximo Reinicio:</gold> <yellow>%time_until_reset_detailed%</yellow>"
  - "<gray>━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</gray>"
  - ""
```

#### Example: French (FR\_FR.yml)

```yaml
# French Translation
prefix: "<gradient:#FFD700:#FFA500><bold>MarchéNoir</bold></gradient> <dark_gray>»</dark_gray>"

no-permission: "%prefix% <red>Vous n'avez pas la permission de faire cela.</red>"
player-only: "%prefix% <red>Seuls les joueurs peuvent utiliser cette commande.</red>"
reload-success: "%prefix% <green>Configuration rechargée avec succès!</green>"
market-reset: "%prefix% <yellow>Le marché a été réinitialisé avec de nouveaux articles!</yellow>"

purchase:
  success: "%prefix% <green>Achat réussi!</green>"
  failed: "%prefix% <red>Transaction échouée. Veuillez réessayer.</red>"
  insufficient-funds: "%prefix% <red>Vous n'avez pas assez d'argent! Requis: <yellow>%price%</yellow></red>"
  out-of-stock: "%prefix% <red>Cet article est en rupture de stock!</red>"

help:
  - ""
  - "<gradient:#FFD700:#FFA500><bold>Aide MarchéNoir</bold></gradient>"
  - "<gray>━━━━━━━━━━━━━━━━━━━━━━━━━━━━━</gray>"
  - "<yellow>/marchenoir</yellow> <gray>- Ouvrir le marché"
  - "<yellow>/marchenoir aide</yellow> <gray>- Afficher cette aide"
  - ""
```

***

### Best Practices

#### 1. Consistent Formatting

Keep consistent styling throughout:

```yaml
# Good - Consistent
prefix: "<gold>[Shop]</gold> <gray>»</gray>"
success: "%prefix% <green>Success!</green>"
error: "%prefix% <red>Error!</red>"

# Bad - Inconsistent
prefix: "&6[Shop] &7»"
success: "<green>Success!</green> %prefix%"
error: "&cError! [Shop]"
```

#### 2. Use Placeholders

Always use placeholders for dynamic content:

```yaml
# Good
insufficient-funds: "%prefix% <red>Need %price% but have %balance%</red>"

# Bad
insufficient-funds: "%prefix% <red>Not enough money!</red>"
```

#### 3. Clear Messages

Make messages clear and actionable:

```yaml
# Good
out-of-stock: "%prefix% <red>This item is sold out! Try again after the next reset.</red>"

# Bad
out-of-stock: "%prefix% <red>Error 404</red>"
```

#### 4. Organize by Category

Group related messages:

```yaml
# Purchase messages
purchase:
  success: "..."
  failed: "..."
  
# Admin messages
admin:
  reset: "..."
  reload: "..."
```

***

### Testing Messages

#### 1. Reload Messages

```
/bma reload messages
```

#### 2. Trigger Messages

Test each message type:

* Buy an item → Purchase messages
* Run admin commands → Admin messages
* View help → Help messages

#### 3. Check Formatting

Verify colors and formatting appear correctly in-game.

***

### Common Issues

#### Issue 1: Colors Not Showing

**Problem**: Messages appear without colors

**Solution**: Check MiniMessage setting

```yaml
# config.yml
messages:
  use-minimessage: true  # Enable for <red> tags
```

#### Issue 2: Prefix Not Working

**Problem**: `%prefix%` shows literally

**Solution**: Ensure prefix is defined

```yaml
prefix: "<gold>[Shop]</gold>"
no-permission: "%prefix% <red>No permission</red>"
```

#### Issue 3: Language File Not Loading

**Problem**: Messages don't change after switching language

**Solution**:

1. Check filename matches exactly (case-sensitive)
2. Verify file is in `lang/` folder
3. Reload: `/bma reload messages`

```yaml
# config.yml - Must match filename exactly
messages:
  lang: "EN_US"  # File: lang/EN_US.yml
```

#### Issue 4: Quotes in YAML

**Problem**: YAML errors with special characters

**Solution**: Quote strings with `%`:

```yaml
# Wrong
message: %prefix% <red>Error</red>

# Correct
message: "%prefix% <red>Error</red>"
```

***

### Multi-Language Setup Example

#### Step-by-Step

**1. Enable localization**:

```yaml
# config.yml
messages:
  localization: true
  lang: "EN_US"
```

**2. Create language files**:

```bash
plugins/BlackMarket/lang/
├── EN_US.yml  # English (default)
├── ES_ES.yml  # Spanish
├── FR_FR.yml  # French
└── DE_DE.yml  # German
```

**3. Configure per-player** (with permission plugin):

```yaml
# Allow players to choose language
# Use with plugin like ChatControl or custom system
```

**4. Switch server language**:

```yaml
# For Spanish server
messages:
  lang: "ES_ES"
```

**5. Reload**:

```
/bma reload messages
```

***

### Advanced: Unicode Characters

#### Symbols

```yaml
prefix: "🛒 <gold>Shop</gold> »"
success: "✓ <green>Success!</green>"
error: "✘ <red>Error!</red>"
info: "ℹ <blue>Information</blue>"
```

#### Emojis

```yaml
purchase:
  success: "💰 <green>Purchase successful!</green>"
  failed: "❌ <red>Purchase failed!</red>"
  
market-reset: "🔄 <yellow>Market refreshed!</yellow>"
```

#### Box Drawing

```yaml
help:
  - "╔═══════════════════╗"
  - "║   BlackMarket     ║"
  - "╠═══════════════════╣"
  - "║ /shop - Open      ║"
  - "╚═══════════════════╝"
```

***

### Message Validation

#### Checklist

* \[ ] All placeholders are correct
* \[ ] Color codes work
* \[ ] Quotes around messages with `%`
* \[ ] No YAML syntax errors
* \[ ] Tested in-game
* \[ ] Consistent formatting
* \[ ] Clear and actionable

#### Validation Tool

Use [YAML Lint](https://www.yamllint.com/) to check syntax.

***

### Next Steps

* [Customize GUI](gui-customization.md)
* [Configure items](items-configuration.md)
* [Learn placeholders](placeholders.md)
* [Set up database](database.md)

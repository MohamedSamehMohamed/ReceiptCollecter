# ReceiptCollecter

An offline Android expense tracker in **Arabic and English**. Record itemized receipts and everyday payments such as rent and subscriptions, explore spending reports, and compare item prices across purchase dates.

## Download and install

**[Download the latest Android APK](https://github.com/MohamedSamehMohamed/ReceiptCollecter/releases/latest/download/receipt-collecter.apk)**

1. Open the link on your Android phone.
2. Download `receipt-collecter.apk` and open it.
3. If Android asks, allow installation from the browser or file manager you used.
4. Tap **Install** for a first installation, or **Update** if the app is already installed.

Keep this link: future releases will use the same APK filename. No USB cable is needed.

Install updates over the existing app to preserve your data. **Do not uninstall first.** Uninstalling or clearing app storage deletes local data.

[Release notes and downloadable files](https://github.com/MohamedSamehMohamed/ReceiptCollecter/releases)

## Features

- **Itemized receipts:** store, buyer, date, notes, categories, quantities, unit prices, and discounts.
- **Quick expenses:** record rent, subscriptions, and other payments with an amount and category; optionally add a description, payee, and notes.
- **Price history:** compare recorded unit prices for the same item across purchase dates and open the related receipts. Prices are shown before discounts.
- **Insights:** spending totals and breakdowns by category, buyer, store/payee, and month, with date filters.
- **JSON import/export:** review imports before confirming and transfer active expenses between installations.
- **Name suggestions:** reuse saved store and item names while entering receipts.
- **Arabic and English**, right-to-left layouts, light/dark themes, and system text scaling.
- **Offline storage:** data stays in the app's local SQLite database. Current expense entry and portable files use EGP.

## Version history

Versions below describe the app's development history. Version **1.4.1** is the first APK published in this GitHub repository; earlier APKs have not been uploaded here.

### 1.4.1 — More expenses per screen

- Reduced default font sizes and shortened the monthly summary card.
- Tightened expense rows, spacing, app bar, and bottom navigation.
- Phone previews at 390 × 844 show four expenses without scrolling in both English and Arabic.
- Preserved system text scaling and the existing database.
- Validation: clean Flutter analysis and all 43 tests passed.

### 1.4.0 — Teal interface and icons

- Introduced a teal theme inspired by the Income & Expense Tracker App Figma community design.
- Added a gradient header, rounded content panels, category icons, and clearer navigation and form icons.
- Split monthly spending into quick expenses and itemized purchases.
- Added a monthly spending trend chart while retaining exact amounts as text.
- Adapted the reference to the app's existing features, with Arabic, dark mode, and large-text support.

### 1.3.0 — Everyday payments and price comparisons

- Added quick expenses for payments such as rent and subscriptions.
- Combined quick payments and receipts in the expense list, Overview, and Insights.
- Added Rent and Subscriptions categories.
- Included item price history with search, dated unit-price observations, price changes, and receipt links.
- Added portable JSON format v2 for mixed quick expenses and itemized purchases; legacy v1 receipt files remain supported.
- Quick expenses are excluded from item price history and item-name suggestions.
- Included an example import template and a Markdown guide for converting receipts into import-ready JSON.

### 1.2.0 — Faster receipt entry

- Added saved-name autocomplete for stores and receipt items.
- Supported Arabic and English matching and removed duplicate suggestions.
- Continued to allow new names; choosing a suggestion fills only the name.
- Required no database schema change.

### 1.1.0 — JSON transfer and discounts

- Added receipt import/export through Settings.
- Added an import preview and confirmation before saving.
- Supported fixed discounts per line and percentage discounts.
- Updated reports to use totals after discounts.
- Added a database migration that preserves existing receipt amounts.

### 1.0.0 — Initial app

- Added manual receipts with categorized items, quantities, and prices.
- Added receipt editing and confirmed deletion.
- Added category, buyer, and store management.
- Added this month's Overview and date-filtered spending Insights.
- Added Arabic/English localization and local SQLite storage.

## Import files

Use the app's **Settings → Import expenses** to select a JSON file, review its contents, and confirm the import. **Export expenses** saves active expenses with their reference names and discounts.

Existing IDs are skipped rather than overwritten. JSON export is an expense transfer format, not a complete database backup. Apps older than 1.3.0 cannot import v2 files containing quick expenses.

The local project includes:
- `examples/expenses.template.json`: an editable example with rent, a subscription, and an itemized purchase.
- `examples/receipt-to-json-guide.md`: instructions you can give another model along with receipt images to generate the JSON.

## Development

This GitHub repository currently hosts the README and APK releases. The Flutter source project is maintained locally; the automatically generated “Source code” release archives contain the repository files, not the complete app source.

With Flutter and the Android SDK installed, run these commands in the source project:

```powershell
flutter pub get
dart run build_runner build
flutter gen-l10n
flutter analyze
flutter test
flutter run
flutter build apk --release
```

On the development machine, local tooling is used by:

```powershell
./scripts/check.ps1
./scripts/build-apk.ps1
```

The current APK uses a development signing key. Published APK updates must keep the same application ID and signing key to install over existing versions.

## Design reference

The 1.4 interface is a visual adaptation of the [Income & Expense Tracker App community Figma design](https://www.figma.com/design/xW78PRQ3IG3bFBGRHY9SRX/Income---Expense-Tracker-App--Community-?node-id=0-1). Exact Figma assets and tokens were not extracted; icons use the app's native icon set.

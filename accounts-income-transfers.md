# Accounts, income and transfers — 1.8.0

Open **Overview → Accounts & money** or **Settings → Accounts & money**.
Create Cash, Bank or Wallet accounts, all in EGP, with an opening balance.
The opening balance is the balance before transactions recorded in this app,
not an extra income entry. Account names, types and opening balances can be edited.
Negative resulting balances are allowed; there is no overdraft enforcement.

Use **Add (+) → Add income** to record salary or other money received into an
account. Use **Transfer money** to move money between two different accounts.
Both flows also appear on Accounts & money. Tap an income/transfer in the activity
list to edit it or delete it with confirmation. Transfers are represented as one
record with a source and destination, so both sides always change together.

When accounts exist, itemized receipts, scan review (including ChatGPT) and quick
expenses show **Paid from**. Choose an account or leave it **Unassigned**.
Existing receipts start unassigned: the upgrade does not guess a payment source,
change totals, delete receipts or change item prices, discounts, categories,
buyers, merchants, notes, dates or photos. Edit a receipt to assign its account.

Balances are calculated from active data:

`opening balance + income + transfers in - transfers out - assigned expenses`

Editing, moving or deleting an expense updates balances automatically; receipt
totals are counted after discounts. Deleting an income/transfer reverses its
effect on balances. Income and transfers do not become receipts or price-history
items. Existing spending reports continue to count only actual expenses.

Accounts & money shows total balances plus this month's income, spending and
income minus spending. Unassigned expenses count in spending, but do not debit
an account. Therefore monthly net income is different from total account balances.
Transfers affect neither monthly income nor spending. All recorded entries,
including future-dated entries, contribute to account balances.

## Existing data and installation

Database schema 3 adds two tables and a nullable receipt account reference.
Upgrade tests cover schema 1 and schema 2 with live and deleted receipts and
items, fixed discounts, notes, image references, timestamps and preferences.
The tests compare every original receipt/item column before and after migration.

Install the signed update over the existing app. **Do not uninstall or clear app
storage first.** Downgrading to an older APK/database schema is not supported.
No user phone/database has been modified by the development tests.

The existing expense JSON import/export format remains compatible. It transfers
expenses only: it does **not** back up accounts, income, transfers or receipt
account assignments. It is not a complete backup of the new financial records.
Imported expenses are unassigned. Phone/cloud backup remains separate work.

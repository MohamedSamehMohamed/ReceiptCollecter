# Receipt scanning with ChatGPT — 1.7.0

1. Open **Add (+) → Scan receipt → Use ChatGPT**.
2. Take a photo or choose one from the device. Crop/rotate if needed.
3. Tap **Use ChatGPT**, then **Send photo to ChatGPT**. The app offers the photo
   and JSON instructions to the official installed Android ChatGPT app. You
   choose the conversation and send the message there. If the text is missing,
   tap **Copy JSON instructions** here and paste it into ChatGPT. If ChatGPT
   cannot be opened, attach the photo and copied instructions manually.
4. Copy ChatGPT's JSON response. Return to ReceiptCollecter and tap **Paste JSON**.
   If Android closed ReceiptCollecter, reopen **Scan receipt → Use ChatGPT →
   Paste JSON / resume**. The last selected photo is retained in private app
   storage; a pasted reply or unfinished editor is not restored after process
   termination, so copy/paste the response again.
5. Tap **Review receipt**. Correct the store, date, items, quantities, unit prices,
   discounts and total. Select buyer and categories. Missing amounts remain blank.
6. Confirm that you reviewed the receipt, then save. Totals must reconcile exactly.
   Editing clears confirmation. JSON parsing and opening review never save data.

No API key is required. ChatGPT needs its normal account/network access. Sharing
uploads the selected receipt to ChatGPT; on-device OCR remains a separate offline
option. ReceiptCollecter does not read chats, select the current conversation,
send the ChatGPT message automatically, or retrieve responses in the background.
Compatibility with ChatGPT's handling of shared text/images requires a phone test.

## Reply format

This is a **single editable scan draft**, separate from the existing portable
expense import/export format. The in-app Copy instructions button contains the
full prompt. The supported object is:

```json
{
  "format": "receipt-collecter-scan",
  "version": 1,
  "currency": "EGP",
  "merchant": "Sample market",
  "date": "2026-10-14",
  "payableTotal": "45.00",
  "notes": "",
  "items": [
    {
      "name": "Milk",
      "quantity": "2.000",
      "unitPrice": "25.00",
      "discountType": "fixed",
      "discountValue": "5.00"
    }
  ]
}
```

- Money and quantities are decimal **strings**; money supports two decimal
  places and quantity three. No floating-point amounts enter storage.
- `discountType` is `none`, `fixed` (whole-line amount) or `percent` (0–100).
  `discountValue` is a string, zero for `none`.
- Merchant, date, payable total, quantity and unit price can be null when unclear.
  Currency can be null when unknown, with an explicit note to confirm EGP during
  review. Other currencies are rejected; no currency conversion is performed.
- All fields shown are required; unknown fields, invalid dates, invalid discounts,
  excessive amounts and more than 200 items are rejected. A complete JSON code
  fence is accepted. Extra prose is rejected.
- Cash tendered, change and gross totals must not replace the payable total.
  Unclear taxes/receipt-level adjustments belong in notes for manual correction;
  the model must not invent a balancing item.

The selected photo is retained locally until replaced, explicitly cleared or a
receipt is saved. The share copy is limited to a dedicated private cache folder
exposed through a non-exported FileProvider with temporary read permission.
Photos are not permanent receipt attachments and are not part of JSON exports.
No database migration or expense transfer format change is introduced.

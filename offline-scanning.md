# Offline receipt scanning — 1.6.0

Open **Add (+) → Scan receipt**. Take a photo or choose one, then crop out the
background and straighten the text. Press **Read receipt offline**. Arabic and
English recognition runs on the Android device using models bundled in the APK.
There is no OCR service, API key, sign-in or runtime model download.

## Review and save

Expand **Receipt photo and recognized text** to compare the source. Correct the
store, purchase date, buyer, item names, quantities, prices, discounts and
categories. Add missed items and remove incorrect proposals. If no usable table
was found, enter the items manually with the photo/text as a reference.

Enter the **payable total printed on the receipt**. It must exactly match the
item total after discounts. Cash tendered, change and gross totals are different
fields. The app does not invent an adjustment item to force a match.

Confirm that every field has been reviewed, then Save. Changes clear that
confirmation. Nothing is saved automatically by recognition. Saved receipts use
the existing schema, JSON transfer and price-history flows.

The photo is a temporary review image. This version does not permanently attach
the image to the saved receipt or include it in JSON export. Canceling review
does not create an expense. Android camera activity recovery is supported for
the photo; an unfinished review draft is not durable across process termination.

## Accuracy and photo quality

This is assisted entry, not guaranteed automatic transcription. Faded thermal
print, folds, glare, fingers covering text and tilted photos produce errors.
Flatten the paper, use a plain contrasting background, good even lighting and
keep all items and totals visible. Crop and straighten before reading.

The private 13-photo/63-item benchmark found that merely switching to native
`tessdata_best` did not improve the baseline. With the original extraction rules:

| Trial | Dates correct | Payable totals correct | Quantity/price pairs correct |
| --- | --- | --- | --- |
| Original JS fast model + automatic cleanup | 7/13 | 1/13 | 5/63 |
| Native best + automatic cleanup | 5/13 | 0/13 | 0/63 |
| Native best + human-inspected paper crops | 4/13 | 3/13 | 2/63 |
| Native fast + human-inspected paper crops | 5/13 | 1/13 | 6/63 |

These are separate desktop experiments, not Android production accuracy. Human
paper-corner crops are user-assisted, and differ from the Android rectangular
crop tool. The app additionally uses evidence-based selection across fast/best
PSM 4/6 and conservative repeated-row arithmetic checks. These changes are
measured separately; do not treat parsed-row counts as complete item accuracy.
On these human-cropped photos, four-trial selection with the exact app parser
produced **5/13 correct dates, 2/13 correct payable totals, 9 proposed rows and
7/63 correct quantity/price pairs**. This is not full item accuracy or Android
runtime accuracy, and shows why manual correction remains essential.
Reference transcription is only used after extraction for scoring. Private
photos, extracted text and reference receipts remain in ignored `imports/`.

## Implementation and licenses

- [Tesseract4Android 4.9.0](https://github.com/adaptech-cz/Tesseract4Android):
  native Tesseract 5.5.1, background single-worker execution.
- [Official fast models](https://github.com/tesseract-ocr/tessdata_fast) and
  [best models](https://github.com/tesseract-ocr/tessdata_best): Arabic (`ara`) and
  English (`eng`). Apache 2.0 license included in APK assets.
- Photo picker: Flutter `image_picker`; crop/rotate: `image_cropper` / uCrop.
- Quantities use integer thousandths; amounts use integer piastres with existing
  exact rounding. No new database tables or migrations.

Bundling both model sets increases APK and installed storage. First scan copies
the models to private app storage. Recognition can take a minute or longer on
slower devices; desktop timing is not a phone-performance prediction.

## Verification status

Flutter analysis is clean; all 62 automated tests pass. Arabic/English light/dark
screens were inspected, with 1.3× text checks. The Android 1.6.0+8 APK builds and
its signature matches existing updates. All models and arm64/armv7/x86_64 OCR
libraries are bundled. The release manifest has **no internet permission**.

Phone runtime verification is pending. No phone was connected; the isolated
software emulator failed to boot without an installed hypervisor.
`integration_test/offline_scan_test.dart` contains a fictional-receipt Android
OCR test ready for a test device; it has not passed yet. Desktop recognition and
Flutter widget tests do not substitute for that device test.

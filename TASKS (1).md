# BUONO: tasks before launch

A working list of what is left before the first live venue. Tick items off as they are done.

Longer-term features are in the To do section of the [README](README.md).

## 1. Repository

- [x] Upload the latest `home.html` (Articles section and four articles).
- [ ] Upload the latest `buono-cards.html` (member numbers on the card fronts).
- [x] Upload the latest `index.html` ("Public page" link in the footer, and the prepayments record).
- [x] Add the licence file (`LICENSE.md`).
- [ ] Optional: add `buono-logo.svg`, the logo without the "accepted here" line.

## 2. Home page content

- [ ] Replace the "list of venues is coming soon" line with the real venues.
- [ ] Add a contact for venues that want to take part.
- [ ] Have a native Italian speaker read the four articles.
- [ ] Check the page on a real phone, in Italian and in English.

## 3. Testing

**Till and wallet**

- [ ] Leave the wallet's device idle for an hour or more, then make a sale.
- [ ] Make a sale with the staff phone on mobile data and the wallet's device on wifi.
- [ ] Restore the wallet on a spare device and confirm the stock counts are still right.

**Gift cards**

- [ ] Print one sheet on plain paper: cards measure 63 mm, and each QR sits behind a logo.
- [ ] Scan a printed QR with a phone to confirm it reads at that size.
- [ ] Claim a gift on a newly installed wallet and pay at the till with only those sats.
- [ ] Try the peel-off stickers: they cover the code fully and come away cleanly.

## 4. Before real money moves

- [ ] Replace the wallet connections used during testing with fresh ones.
- [ ] Publish real prices and prepaid counts from the dashboard.
- [ ] Record a first sweep, so takings start from zero without the test payments.
- [ ] Store the `bvadmin1:` backup line somewhere safe, away from the admin device.
- [ ] Store the wallet's recovery phrase somewhere safe, away from the admin device.

## 5. Advice to take

- [ ] Fiscal: how the prepayment to a venue and the sats received are recorded and taxed.
- [ ] Regulatory: the rules that apply to exchanging cash for sats, before any exchange takes place.

## 6. With the first venue

- [ ] Agree the product, the price and the size of the first batch.
- [ ] Agree how the venue's own till records an item that has already been paid for.
- [ ] Buy a duplicate pad of generic receipts (*ricevute generiche*) for the venue.
- [ ] Prepay the batch: the venue signs a slip from the pad and gives its till receipt; staple the two together.
- [ ] Record the top-up in the dashboard with the euros paid, the slip number and the till receipt number.
- [ ] Print the staff QR and brief the staff on taking a payment.
- [ ] Print and place the window sticker.
- [ ] Add or update the venue's BTC Map listing.
- [ ] Run one real sale together before opening it to customers.

## 7. Documentation

- [ ] Add gift cards and the card maker to the guide, once the print test has passed.
- [ ] Update the README's "tested" list to include the gift flow and the home page.

## Known limits accepted at launch

These are documented in the README and are not blockers.

- Wallets that keep one history for all connections let the holder of one venue's staff link read other venues' sales. Counts are unaffected.
- Two phones charging in the same second could both sell the last item.
- Tills pick up price and stock changes the next time they are opened.
- The signed settings are carried by third-party relays.

# BUONO

<p align="center"><img src="buono-sticker.svg" width="260" alt="BUONO"></p>

A single-file web app for running a community voucher scheme paid in bitcoin.

An organiser prepays a local venue for a fixed number of items, such as ten pizzas. Customers then pay for those items in sats over Lightning, at the venue, at the day's exchange rate. The venue has already been paid in its own currency and never has to touch bitcoin. The sats go to the organiser's wallet, and a counter shows how many prepaid items are left.

There is no server, no account and no database. The app is one HTML file, `index.html`, with a public home page, `home.html`, beside it. Both can be hosted on any static web host.

> **Status: pilot.** The app has been tested end to end with real Lightning payments using Blitz Wallet. It has not yet been used in a live venue. See [Status and testing](#status-and-testing) and [Known limits](#known-limits) before relying on it.

## How it works

There are three roles.

| Role | What they do | What they need |
|---|---|---|
| **Organiser** | Prepays venues, receives the sats, sets prices and stock | A Lightning wallet with Nostr Wallet Connect, and one device kept as the admin device |
| **Venue** | Hands over the goods and takes payment with the till | Any phone with a browser |
| **Customer** | Pays in sats | Any Lightning wallet |

A sale goes like this:

1. A customer asks to pay in bitcoin. In practice this works like splitting a bill: the rest of the table pays by card or cash, and the bitcoin share is paid in sats.
2. The waiter opens the till from the staff link, chooses the quantity and taps **Charge**.
3. The till asks the organiser's wallet for a Lightning invoice for the item's price, converted to sats at the current rate, and shows it as a QR code.
4. The customer scans and pays.
5. The till shows a green **Paid** screen and the stock counter drops.

## Features

**Till**

- One screen per venue, with a counter of prepaid items remaining.
- Fixed price in local currency, converted to sats at the moment of sale.
- Several items per venue, each with its own price, stock and maximum per sale.
- No overdraft: stock is checked before every invoice, and the till stops at zero.
- Italian and English, switchable on screen.

**Dashboard**

- Stock and sales per venue.
- Venue details, items, prices and top-ups edited on the admin device and published as signed settings.
- A dated change record of every price change, top-up and new item.
- Takings since the last sweep across all venues, with a sweep history and carry-over.
- Several venues, each with its own wallet connection and staff link.
- Backup and restore of the admin key and connections as a single line of text.

**Home page**

- A public page in Italian and English explaining the scheme to gift-card holders, customers and venues.
- Shown to any visitor who arrives without a staff link; the organiser's setup sits behind a link at its foot.
- Plain HTML, so the wording is easy to edit.

**Setup**

- Wallet connection entered by pasting, or by reading a saved image of its QR code.
- The connection is checked on entry and refused if it is able to spend.
- A printable staff QR per venue.
- A demo mode with no wallet: open the page with `?demo` on the end.

## Design

**The wallet is the ledger.** Every invoice description ends with a short tag, for example `[bv:pizzeria1:special:2:2400]`, giving the venue, item, quantity and price. The till counts sales by reading the wallet's own transaction history over Nostr Wallet Connect. Nothing is stored anywhere else, so any phone that opens the till arrives at the same count.

**Connections are receive-only.** The till needs three NWC permissions: `make_invoice`, `lookup_invoice` and `list_transactions`. It never needs to send, and it refuses a connection that reports it can.

**Secrets stay out of the file.** The wallet connection travels in the staff link after the `#`, which browsers do not send to the web host. The hosted file contains nothing secret and can live in a public repository.

**Settings are signed.** Venue details, prices and prepaid totals are published by the organiser as signed Nostr notes (kind 30078, one replaceable note per venue). Tills read the newest note signed by the organiser's admin key and ignore anything else. Staff can read the settings but cannot change them. A till that cannot verify any settings refuses to charge.

**The admin key stays on one device.** It is created in the browser on the organiser's device and kept in that browser's storage, together with the wallet connections. Staff links carry only its public half.

**Takings records are private.** Sweep history is kept on the admin device and in its backup line. It is never published.

## Requirements

- A static web host with HTTPS, such as GitHub Pages.
- A Lightning wallet that supports Nostr Wallet Connect (NIP-47) with the three permissions above, and that stays reachable while venues are open.
- A modern mobile browser on the admin device and on each staff phone.

The page loads four libraries from a public CDN at pinned versions: `qrcode-generator` 1.4.4, `@getalby/sdk` 8.0.3, `nostr-tools` 2.25.2 and `jsqr` 1.4.0. Each carries an integrity value, so the browser refuses a file that has been altered. It reads the exchange rate from mempool.space, with Coinbase as a fallback, and publishes settings to the relays listed in the config.

## Getting started

See **[GUIDE.md](GUIDE.md)** for the full walkthrough, from forking the repository to sweeping the takings.

To look around without a wallet, add `?demo` to the app's address, for example `https://YOUR-NAME.github.io/buono-btc/?demo`.

## Configuration

A small block at the top of `index.html` holds the scheme-wide settings. Venues, items and prices are managed in the dashboard, not here.

| Setting | Meaning |
|---|---|
| `scheme` | Name shown on every screen and in the browser tab |
| `schemeId` | Internal id used to find the published settings. Choose it before first use; changing it later means republishing every venue |
| `currency` | Local currency code used for prices, for example `EUR` |
| `lang` | Default language, `it` or `en` |
| `invoiceMinutes` | How long a payment QR stays valid |
| `pollSeconds` | How often the till asks the wallet whether an invoice is paid |
| `walletWaitSeconds` | How long to wait for a sleeping phone wallet to answer |
| `lowStockAt` | Stock level at which the dashboard shows a warning |
| `ntfyTopic` | Optional ntfy.sh topic for a phone alert on each sale |
| `priceApi` | Exchange-rate source; any mempool instance works |
| `homePage` | The page a visitor without a staff link is sent to. Leave empty to show the organiser setup instead |
| `relays` | Nostr relays that carry the signed settings |
| `adminPubkey` | Recommended: pins your admin key, so that every link follows your settings. The dashboard shows the line to paste |
| `venues` | Normally empty. Can hold starting values for a venue with nothing published yet |

## Status and testing

Tested with real payments, using Blitz Wallet on one tablet and the till on a separate phone:

- Setup, staff QR, charging, payment detection and the stock counter.
- Cancelled and expired invoices.
- The wallet answering while in the background, swiped away, and with the tablet locked.
- Signed settings on public relays: prices, top-ups, venue details and new items reaching a staff phone.
- A second venue with its own connection, entered from a QR image.

Not yet tested:

- The wallet device left idle for an hour or more before a sale.
- The till and the wallet on different networks.
- Wallets other than Blitz.
- A live venue during service.

## Known limits

- **Changes are picked up when a till is next opened**, not while it sits open.
- **Two charges in the same second** on the last item could both get a QR. Closing this fully would need a server.
- **A staff link is a key.** Whoever holds it can create invoices to the organiser's wallet and read that connection's incoming history. They cannot spend. Deleting the connection in the wallet revokes it.
- **Wallets that share one history across connections**, as Blitz does, let a technically able holder of one venue's link read other venues' sales. Counts are unaffected. A wallet with a separate sub-account per connection avoids this.
- **Some wallets list only completed payments.** With those, a second phone cannot see an invoice that is waiting to be paid, so reservations are only visible on the phone that made them.
- **The till runs on the staff phone.** Signed settings stop casual changes, but a determined person could alter their own copy of the page. The dashboard lists the price of every sale, so odd amounts are visible.
- **Relays are third parties.** If none is reachable and a phone has no remembered settings, the till will not charge.
- **No refunds through the app.** A refund is handled by the venue and settled with the organiser.
- **The app does not handle the local-currency side.** Prepaying venues, receipts and tax treatment are the organiser's responsibility and differ by country. Take advice before running a scheme.

## Window sticker

`buono-sticker.svg` is a round sticker for the window or till of a participating venue. It is a vector file and prints at any size; 10 cm across or larger reads well from the street. Organisers are free to print it for their venues.

## To do

- [ ] **BTC Map as the verification layer.** Each venue's listing is maintained by the local organiser on their BTC Map account. The dashboard stores a link today; checking a venue against its listing is still to build.
- [ ] **Venues on the home page.** The home page explains the scheme today, with venues listed by hand. Still to build: listing participating venues and their products autom
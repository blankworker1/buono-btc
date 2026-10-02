# BUONO: setup and use

A step-by-step guide for an organiser, from forking the repository to sweeping the takings.

The app's screens are in Italian by default, with an **English** button at the bottom of every page. This guide gives the Italian label first and the English one in brackets.

## Contents

1. [What you need](#1-what-you-need)
2. [Fork and host the app](#2-fork-and-host-the-app)
3. [Set the scheme-wide options](#3-set-the-scheme-wide-options)
4. [Create a wallet connection](#4-create-a-wallet-connection)
5. [Set up the first venue](#5-set-up-the-first-venue)
6. [Back up the admin key](#6-back-up-the-admin-key)
7. [Add items and publish](#7-add-items-and-publish)
8. [Prepay the venue](#8-prepay-the-venue)
9. [Give the venue its staff QR](#9-give-the-venue-its-staff-qr)
10. [Taking a payment](#10-taking-a-payment)
11. [Day-to-day changes](#11-day-to-day-changes)
12. [Adding more venues](#12-adding-more-venues)
13. [Reconciling and sweeping](#13-reconciling-and-sweeping)
14. [Backup, restore and revoking access](#14-backup-restore-and-revoking-access)
15. [Before going live](#15-before-going-live)
16. [Troubleshooting](#16-troubleshooting)

---

## 1. What you need

- **A GitHub account**, or any other static web host with HTTPS.
- **A Lightning wallet with Nostr Wallet Connect (NWC).** This guide uses Blitz Wallet, which is what the app has been tested with.
- **An admin device.** A tablet or phone that you keep. It holds the admin key and the wallet connections, and it is the only device that can change prices and stock.
- **A staff phone at each venue.** Any phone with a browser and a camera.
- **A second Lightning wallet** for test payments.

The wallet must stay reachable while venues are open. Each sale asks it to create an invoice and to confirm payment. A phone wallet does this through push notifications, so its device needs to be charged, online, and allowed to show notifications.

## 2. Fork and host the app

1. On GitHub, open the BUONO repository (`buono-btc`) and choose **Fork**.
2. In your fork, go to **Settings**, then **Pages**.
3. Under **Build and deployment**, set **Source** to "Deploy from a branch", choose the `main` branch and the root folder, and save.
4. After a minute or two, the app is live at an address like `https://YOUR-NAME.github.io/buono-btc/`.

The page must be opened from its web address. A copy opened from a downloaded file works on that device only, and its staff QR will not open on other phones.

The hosted file contains nothing secret, so a public repository is fine. Browser storage is shared by every page under the same `github.io` address, so only host code you trust there.

## 3. Set the scheme-wide options

Edit `index.html` in your fork (the pencil icon on GitHub) and find the `CONFIG` block near the top. Set at least:

- `scheme`: the name of your scheme, shown on every screen. You can change it at any time.
- `schemeId`: a short internal id in lowercase, for example your town's name. Choose it now and leave it alone: the published settings are filed under it, and changing it later means republishing every venue from the dashboard.
- `currency`: your local currency code, for example `EUR`.
- `lang`: `it` or `en`.

Venues, items and prices are created in the dashboard, not in the file, so the `venues` block stays empty.

Commit the change. GitHub Pages updates within a few minutes.

To see the screens before connecting a wallet, add `?demo` to the address. Demo mode takes no real payments.

## 4. Create a wallet connection

Create one connection for each venue. In Blitz:

1. Open the Nostr Wallet Connect section and create a new connection named after the venue.
2. Switch **on**: Receive payments, Transaction History, Lookup Invoice.
3. Switch **off**: Send payments, Get Balance.
4. Take a screenshot of the connection's QR code, or copy the connection string.

In other wallets the permissions are called `make_invoice`, `list_transactions` and `lookup_invoice`. The app refuses a connection that reports it can send.

Blitz keeps all NWC connections in one sub-wallet, separate from your main balance, with a button to withdraw to the main wallet.

## 5. Set up the first venue

On the admin device, open the app's web address. The setup screen appears.

1. Under **Locale** (Venue), choose **+ Nuovo locale…** (+ New venue…) and type the venue's name.
2. Enter the connection in one of two ways:
   - paste the connection string into the box, or
   - tap **Leggi la connessione da un'immagine QR** (Read the connection from a QR image) and pick the screenshot from the gallery.
3. Tap **Crea il link per lo staff** (Create the staff link).

The app checks the connection, creates your admin key, and stores both on this device. From now on, opening the app on this device goes straight to the dashboard with nothing to paste.

If a warning says the connection can also send payments, go back to the wallet and switch sending off for that connection, then try again.

Delete the screenshot once the venue is set up. It contains the connection.

## 6. Back up the admin key

Do this before anything else.

1. Tap **Vai al riepilogo** (Go to the dashboard).
2. Scroll to **Chiave admin e backup** (Admin key and backup) and open it.
3. Copy the line that starts with `bvadmin1:` and keep it somewhere safe and private.

That line restores the admin key, the wallet connections and the sweep history on another device. Whoever has it can change your prices and stock. If the browser's data is cleared and you have no backup, you lose the ability to change settings for venues already running.

**Pin your key in the file.** The same panel shows a line starting `adminPubkey:`. Paste it into the `CONFIG` block in the file, replacing the `adminPubkey` line that is there, and commit. Every staff link then follows your settings, and a link altered to point at a different key is ignored.

A forked copy may still carry the key of the organiser it was copied from. Until you replace it, your tills follow their settings and your dashboard has no settings controls. The dashboard shows a red warning with the correct line whenever the key in the file is not the one on your device.

## 7. Add items and publish

In the dashboard, under **Impostazioni** (Settings):

1. Check the venue name.
2. Write the **Messaggio sulla ricevuta del cliente** (Message on the customer's receipt). The line underneath shows what the customer's wallet will display.
3. Optionally paste the venue's BTC Map link.
4. Tap **Aggiungi un prodotto** (Add an item) and fill in:
   - **Nome** (Name)
   - **Prezzo** (Price) in your local currency
   - **Massimo per vendita** (Maximum per sale): how many can be paid for with one QR
   - **Prepagati iniziali** (Prepaid to start with): how many you have prepaid
5. Tap **Firma e pubblica** (Sign and publish).

The message should say the change was published to at least one relay. If it says no relay accepted it, check the connection and try again.

A venue's staff link does not work until its first item has been published.

## 8. Prepay the venue

This part happens outside the app. Agree the item and its price with the venue, pay them for the batch in the local currency, and keep the paperwork. The number you enter as prepaid in the dashboard should match what you have actually paid for.

Agree with the venue how the prepaid item is recorded on their till when a customer claims one, since no money changes hands at that moment.

## 9. Give the venue its staff QR

1. In the dashboard, tap **QR e link dello staff** (Staff QR and link).
2. Tap **Stampa il QR dello staff** (Print the staff QR), or open the link on the venue's phone and add it to the home screen.

Treat the staff QR like a key. Keep it behind the counter, not on tables or in the window. Whoever has it can create payment requests to your wallet and see that connection's takings. They cannot spend.

The staff QR opens the till. It is not the payment QR, which appears on screen at each sale and is different every time.

## 10. Taking a payment

For the waiter:

1. A customer asks to pay in bitcoin. Split the bill as usual; the bitcoin share covers the prepaid item.
2. Scan the staff QR with the phone camera. The till opens and shows how many are left.
3. Choose the quantity and tap **Incassa** (Charge).
4. Show the customer the payment QR on screen.
5. Wait for the green **Pagato** (Paid) screen. If the customer says they have paid and the screen has not changed, tap **Controlla ora** (Check now).
6. Mark that share of the bill as already paid on the venue's own till.

A payment QR is valid for three minutes. **Annulla** (Cancel) returns to the till. When stock reaches zero the till shows **Esaurito** (Sold out) and stops charging.

The first screen may take a few seconds to fill in after a quiet period, while the wallet wakes up.

## 11. Day-to-day changes

All of these are done in the dashboard on the admin device, followed by **Firma e pubblica** (Sign and publish).

| To do this | Change this |
|---|---|
| Top up after prepaying more | Enter the number in **Ricarica** (Top-up) |
| Correct a top-up mistake | Enter a negative number in **Ricarica** |
| Change a price | Edit **Prezzo** |
| Take an item off sale | Switch off **In vendita** (On sale) |
| Rename a venue or item | Edit the name |

Items are taken off sale, never deleted, so past sales stay in the record.

Tills pick up changes the next time they are opened. Every change is listed with its date under **Registro delle modifiche** (Change record), which staff can read but not edit.

## 12. Adding more venues

1. Create a new connection in the wallet, as in step 4.
2. In the dashboard, tap **Aggiungi un locale** (Add a venue).
3. Name it, enter its connection, and create its staff link.
4. Open its dashboard, add its items and publish.

A menu at the top of the dashboard switches between venues. Each venue has its own staff QR, stock and change record.

## 13. Reconciling and sweeping

The dashboard on the admin device has a section called **Incassi da trasferire** (Takings to sweep). Staff phones do not see it.

1. Open the dashboard. With several venues, tap **Aggiungi gli altri locali al totale** (Add the other venues to the total).
2. Compare **attesi nel wallet** (expected in the wallet) with the NWC balance shown in the wallet app.
3. In the wallet app, move the NWC funds to your main wallet.
4. Back in the dashboard, check the amount under **Sats trasferiti al wallet principale** (Sats moved to the main wallet). It is filled in with the expected figure; change it if you moved a different amount.
5. Tap **Registra il trasferimento e azzera** (Record the sweep and reset).

The takings counter restarts from zero. Stock and prepaid counts are not affected.

- Any difference between what was expected and what you moved is carried into the next period.
- Sales from the last two minutes are left for the next period, so a payment still arriving is not miscounted.
- **Storico dei trasferimenti** (Sweep history) lists every sweep and has an undo for the last one.
- The history is stored on the admin device only. Take a fresh backup line after sweeps you want to keep.

## 14. Backup, restore and revoking access

**Restore on a new device.** Open the app, open **Ho un backup da ripristinare** (I have a backup to restore) on the setup screen, paste the `bvadmin1:` line and tap **Ripristina** (Restore).

**A staff QR is lost, or someone leaves the staff.** Delete that venue's connection in the wallet, which cuts the old link off at once. Create a new connection, open **QR e link dello staff**, replace the connection in the box with the new one, tap **Crea il link per lo staff**, and print the new QR.

**Stop using a device as admin.** In **Chiave admin e backup**, tap **Rimuovi chiave e connessioni da questo dispositivo** (Remove the key and connections from this device). Make sure you have the backup line first.

## 15. Before going live

Run these with a low test price and a second wallet to pay from.

- [ ] One real payment: green screen, counter drops, sale listed in the dashboard.
- [ ] Reopen the till from scratch and check the count is unchanged.
- [ ] Let one payment QR expire, and cancel another. Stock should return both times.
- [ ] Charge with the wallet app in the background, swiped away, and with its device locked.
- [ ] Charge after the wallet's device has been idle for an hour or more.
- [ ] Charge with the staff phone on mobile data and the wallet's device on wifi.
- [ ] Publish a price change and confirm the staff phone shows it after reopening.
- [ ] Record a first sweep, so the takings counter starts clean.
- [ ] Set the real prices and prepaid counts.
- [ ] Replace any connection that was shared or pasted somewhere during testing.

## 16. Troubleshooting

| What you see | Likely cause | What to do |
|---|---|---|
| The connection "can also send payments" | Sending is switched on for that connection | Switch it off in the wallet and recreate the link |
| "The connection is missing a permission" | Receive, history or lookup is off | Switch all three on in the wallet |
| "The wallet did not answer in time" | The wallet's device is off, offline, or blocking notifications | Check it is online, allow notifications, set battery use to unrestricted |
| The price on the staff phone has not changed | The till was already open, or the link predates the admin key | Reopen the till; if still wrong, rescan the staff QR |
| "The signed settings could not be read" | No relay reachable and nothing remembered on that phone | Check the phone's connection and reopen |
| "This venue has no published settings yet" | Nothing has been published for the venue | Add an item in its dashboard and publish |
| "No readable QR code in this image" | The image is blurred or cropped | Use a screenshot showing the whole code |
| A red note says the admin key in the file is not this device's | The file pins another organiser's key, or the device was set up with a different key | Paste the `adminPubkey` line shown in the note into the file's config |
| The staff QR opens an unrelated app | The page was opened from a downloaded file | Open the app from its web address and recreate the QR |
| Lots of wallet notifications during a sale | Some wallets notify on every request | Raise `pollSeconds` in the config and rely on **Controlla ora** |
| The wallet holds more than the dashboard expects | A payment arrived outside the till, or after the last sweep was recorded | Enter the amount actually moved when recording the next sweep |

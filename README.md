# Manufacturing & Invoicing Manager (single-file, black &amp; white, password-locked)

A dashboard for a small business that makes what it sells: one product
catalog that separates raw materials from finished goods, a Bill of
Materials on every finished good, a production/manufacturing page that
turns materials into finished stock, and invoicing that pulls prices
straight from that same catalog and deducts finished-goods stock the
moment an invoice is sent. Runs entirely in the browser, no build step,
no npm, no server. Locked behind a simple access code you set yourself.

This is a brand-new, sixth product — built by merging what a small
manufacturer/maker actually needs (invoicing + inventory + a
production step in between), researched against how QuickBooks
Manufacturing, Zoho Inventory, and Katana handle the same problem
before deciding what to build and what to deliberately leave out. The
five existing apps (Property Management, Invoice Generator,
Appointment Booking, Inventory Management, Stock & Invoicing Manager)
are unaffected and still available on their own.

## What's in this folder

15 pages plus this README — no folders, nothing to lose during upload:

```
index.html               ← Dashboard (open this one first)
products.html             ← Materials & finished goods catalog
bom.html                    ← Bill of Materials editor
production.html               ← Turn materials into finished stock (Simple or Advanced)
vendors.html                    ← Supplier directory
purchase-orders.html              ← All purchase orders, search/filter, print
po-editor.html                      ← Create/edit a purchase order
accounts-payable.html                 ← What you owe vendors, record vendor payments
customers.html                          ← Customer directory
invoices.html                             ← All invoices, search/filter, payments, print
editor.html                                 ← Create/edit an invoice, pick items, send, WhatsApp
credit-notes.html                             ← Issue credit notes, apply them to invoices
payments.html                                   ← Full payment history
reports.html                                      ← Revenue, production output, material usage
settings.html                                       ← Business profile, tax ID, invoicing, backup
README.md                                             ← This file
```

## Setting your customer's access code — do this before you hand it over

Every visitor has to type a code before they can see anything. Right now
that code is set to `mfg2026`, and you need to change it before
publishing, or anyone who has this README can get in.

1. Open `index.html` in a plain text editor (Notepad, TextEdit, VS Code —
   anything works).
2. Press Ctrl+F / Cmd+F and search for `mfg2026`.
3. Replace it with whatever code you want to give this customer — for
   example `"smith-jan-2026"`. Keep the quote marks around it.
4. **Repeat this in all 15 `.html` files** — each page checks its own copy
   of the code, so if you skip one, that page will still ask for the old
   code. (This is quick to do with your editor's "Find in Folder / Replace
   in Files" feature if it has one — search for `mfg2026` across all 15
   files at once and replace every occurrence.)
5. Save all 15 files, then upload them to GitHub as normal.

Give your customer the web address plus the code you chose. The first
time they visit, they'll see an "Enter Access Code" screen; once they type
it correctly, that browser stays unlocked (it won't ask again on that
device unless they clear their browser data).

**Be honest with yourself about what this does and doesn't do:** this app
has no server, so the code lives right inside the page. Anyone who opens
their browser's "View Page Source" can read it in plain text. It's a real
deterrent against a customer sharing the link casually or a stranger
stumbling onto it — not a lock that would stop someone determined to get
in. Don't put anything you'd consider truly sensitive behind it.

## Try it on your computer right now

Double-click `index.html`, type in whatever code is currently set (the
default is `mfg2026` until you change it per the steps above), and
you're in.

## Publish it for free on GitHub Pages

1. Open your repository on GitHub.
2. Click **Add file → Upload files**.
3. Drag in all 15 `.html` files plus this `README.md`. Check the box shows
   exactly those files before committing — no folders.
4. Click **Commit changes**.
5. Go to **Settings → Pages**. Set **Source** to **Deploy from a branch**,
   branch **main**, folder **/ (root)**, then **Save**.
6. Wait about a minute, then visit
   `https://<your-github-username>.github.io/<repository-name>/`.

**To update the site later:** edit a file, drag it back into **Add file →
Upload files** to replace it, and commit.

## Materials, finished goods, and the Bill of Materials

The **Products** page holds two kinds of item in one catalog, switchable
with a filter at the top:

- **Raw materials** — the things you buy in (fabric, screws, resin,
  packaging). They have a stock quantity, cost price, and reorder
  threshold, but no sell price and never appear on an invoice directly.
- **Finished goods** — the things you sell. Each one carries a **Bill of
  Materials**: a list of raw materials and how much of each is consumed
  per unit produced. Finished goods have both a cost and a sell price,
  the sell price being what auto-fills on an invoice line when it's
  picked.

Only finished goods appear in the invoice line-item picker. Materials
are consumed exclusively by production — never deducted directly by an
invoice.

## How production works — Simple and Advanced modes

**Settings → Production Mode** (also switchable right from the
Production page itself) picks between two ways of turning materials
into finished stock, so you can start simple and switch to Advanced
later without losing anything:

- **Simple mode** — pick a finished good, enter a quantity, click
  Produce. If every material in its BOM has enough stock, the
  materials are deducted and the finished good's stock goes up, all in
  one step, logged instantly to the Recent Production table. This is
  the right mode if you make things to order in a single pass and
  don't need to track a job in progress.
- **Advanced mode** — a Kanban board (Queued → In Progress → Done) for
  running production as actual work orders. Create a work order for a
  quantity of a finished good, **Start** it when work begins, and
  **Complete** it when it's done — materials are deducted and finished
  stock is added only at the moment a work order is marked complete,
  not when it's created or started. This is the right mode if
  production takes real time and you want to see what's in progress at
  a glance, or **Cancel** a work order that never gets made without
  touching stock at all.

Either way, there is exactly one function in the app that ever changes
finished-goods or materials stock for production — this is deliberate,
the same discipline the invoice-stock deduction already follows, so
stock numbers can never drift out of sync with what actually happened.

## Credit notes

The **Credit Notes** page lets you issue a credit against a customer —
for a discount, refund, damaged goods, return, or billing correction —
independently of any single invoice, the same way QuickBooks, Xero,
and Zoho Invoice handle it. Issue the credit note first, then use
**Manage** to apply some or all of it to one or more of that
customer's sent invoices as a separate step; a credit note can be
split across several invoices, and an invoice can have credit from
more than one credit note applied to it. Applying credit reduces an
invoice's balance due exactly like a payment does, and shows as its
own line — separately from Paid — on the invoice editor, the invoices
list, and the printed invoice. A credit note can be edited or deleted
only while nothing has been applied yet; once any amount is applied,
remove the application first (or issue a fresh credit note instead) —
this keeps the numbers always trustworthy rather than editing
something a past application already depended on. Deleting an invoice
that has credit applied to it automatically frees that credit back up
on the credit note.

## How the invoice lifecycle works

- **Draft.** A new invoice starts as a draft. Drafts are completely
  stock-neutral — nothing is reserved or deducted. Every field,
  including line items, is fully editable.
- **Sending.** Clicking **Save & Send** checks every finished-goods
  line item against current stock, all at once. If everything has
  enough stock, the invoice is marked Sent and stock is deducted for
  every line in one atomic step. If anything is short, nothing is
  deducted — you'll see exactly which items and by how much, and the
  invoice you were editing is saved as a draft so your work isn't
  lost.
- **Locked after sending.** Once an invoice is Sent, its customer,
  dates, and line items become read-only in the editor. To change what
  was actually sold: **Cancel** the invoice (this restores the stock
  it deducted), then either **Reopen** it as an editable draft or
  **Duplicate** it into a brand-new draft.
- **Status** (Draft / Sent / Partial / Paid / Overdue / Cancelled) is
  derived automatically from payments, credit applied, and dates — you
  never set it directly except via the Save Draft / Save & Send /
  Cancel / Reopen buttons.
- **Print / Save as PDF** opens a clean, printable version of the
  invoice in a new tab and triggers your browser's print dialog —
  choose "Save as PDF" as the destination. This works completely
  offline, no external library required.

## Business profile, tax registration, and vendor tax IDs

**Settings → Business Profile** is where your logo, business name,
address, and contact details live, printed on every invoice, credit
note, and purchase order letterhead. Right below it, **Tax / VAT
Registration** holds your own business's registration number —
whatever your jurisdiction calls it (VAT No., GSTIN, EIN, Company
Reg. No., ABN…). Both the label and the number are yours to set;
leave the number blank and it's simply left off printed documents.
Settings itself is organized into tabs — Business Profile, Invoicing
& Documents, Production & Inventory, Appearance, and Backup & Data —
so each screen stays focused instead of one long scroll.

Vendors carry their own **Tax / VAT / Registration No.** field too
(Vendors → Add/Edit Vendor), which prints on that vendor's purchase
orders and appears in the Excel export's Vendors sheet.

## Sending invoices and credit notes via WhatsApp

Next to **Print / Save as PDF** on the invoice editor, the invoices
list, and the credit notes list, a **Send via WhatsApp** button builds
a real PDF of that document and hands it to WhatsApp two different
ways depending on what the visitor's browser supports — these are the
only two mechanisms any web page has for this, not a limitation of
this app specifically:

- **On most phones** (where the browser supports sharing files), it
  opens the native share sheet with the PDF already attached — picking
  WhatsApp there sends the real file in one tap.
- **On most desktop browsers** (which don't yet support sharing files
  from a web page), the PDF downloads automatically and a WhatsApp
  chat opens pre-filled with a short message, ready for that
  just-downloaded file to be dragged in. WhatsApp's own chat links
  only ever accept text, never a file attachment — that's a platform
  limit, the same reason the Email button next to it asks you to
  attach the downloaded PDF by hand too.

The number it messages is that customer's or vendor's **Phone** field
(Customers/Vendors → Add/Edit) — add one there if a WhatsApp chat
isn't opening. Building the PDF itself needs an internet connection
the first time it's used per browser session (it loads a small,
widely-used PDF library, jsPDF, from a public CDN) — everything else
in this app, including Print/Save as PDF, keeps working completely
offline either way.

## What this app deliberately leaves out of v1

- **No multi-level Bills of Materials.** A finished good's BOM lists
  raw materials directly — a finished good can't be a component inside
  another finished good's BOM. Most small manufacturers have one
  production stage; nested BOMs (sub-assemblies) are a real feature
  some businesses eventually need, but they add real complexity and
  weren't worth bolting on speculatively.
- **No scrap/wastage or yield tracking.** Production assumes the
  quantity you ask for is the quantity you get — there's no separate
  accounting for spoiled units or a yield percentage below 100%.
- **No labor costing or routing steps.** A work order tracks
  Queued/In Progress/Done, not a sequence of operations or the labor
  time/cost that went into each one.
- **No recurring invoices.** Same reasoning as Stock & Invoicing
  Manager: recurring stock-linked invoices raise questions (what
  happens when a recurring run hits a shortage?) worth answering
  deliberately rather than bolting on. If a customer needs it, it can
  be added later.

Any of these can be added later if a customer actually needs it — they
were left out on purpose, not by accident.

## Data & privacy

All data lives only in *that visitor's* browser local storage — nothing
is sent anywhere, no account, no login beyond the access code.

- Data does **not** sync between devices, browsers, or visitors — each
  person who unlocks the app has their own separate data.
- Clearing browser site data erases it.
- **Settings → Backup & Data → Export backup (.json)** downloads/restores
  a `.json` backup containing everything: materials, finished goods,
  BOMs, production orders, vendors, purchase orders, customers,
  invoices, credit notes, and payments. Use this to move data between
  devices or restore after clearing browser data.
- **Settings → Backup & Data → Export All Data (Excel)** downloads a
  nicely formatted `.xlsx` workbook with your business name in the file
  name — a cover sheet with your business details (including your Tax
  / VAT registration number), plus separate sheets for items, BOM
  lines, production orders, vendors, purchase orders, purchase order
  line items, customers, invoices, invoice line items, credit notes,
  payments, and stock movements. This is for reporting/sharing/
  printing, not for restoring data into the app — use the `.json`
  backup for that. It's built entirely on-device with no CDN
  dependency, so — like production and printing invoices — it works
  completely offline. (Sending a document via WhatsApp is the one
  feature in this app that does need an internet connection the first
  time it's used per session — see above.)

## Customizing

Each page carries its own `<style>` and `<script>` blocks — open any file
in a text editor to tweak colors, wording, or logic. No build tools
needed.

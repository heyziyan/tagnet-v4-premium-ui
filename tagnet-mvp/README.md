# Tagnet MVP — Self-Activation + Dark Mode

Local Node.js/Express MVP for Tagnet NFC keychains. This version adds customer self-activation: the NFC chip opens a stable Tagnet URL, and the buyer claims the tag using a one-time code included in the package. Customers do not need NFC Tools or a separate app.

## Run locally

Requires Node.js 18 or newer.

```bash
npm install
npm start
```

Open `http://localhost:3000`.

Fresh local demo accounts (only seeded if the database is empty):

- Admin: `admin@tagnet.local` / `TagnetAdmin123!`
- Demo customer: `demo@tagnet.local` / `DemoUser123!`

Routes:

- `/` — homepage
- `/login` — login
- `/signup` — customer signup
- `/dashboard` — customer profile editor and QR
- `/products` — product catalog
- `/admin` — admin dashboard, customer data, tags and product manager
- `/health` — health check

## Prepare an order for customer activation

**Use the deployed HTTPS domain for real NFC chips. Do not write localhost URLs to a keychain that will be mailed to a customer.**

1. Log in as admin and open `/admin`.
2. Under **Prepare a new NFC keychain**, click **Generate tag + activation code**.
3. Save the Tag ID, the 8-character activation code, and the displayed NFC URL. The activation code is shown only once after creation; copy it immediately.
4. Write only the NFC URL to the physical chip. Example: `https://your-real-domain.com/t/TGABC123`.
5. Put the one-time activation code on a small printed card inside the package. Do not write the activation code to the NFC chip.
6. Test the chip with a phone before shipping. It should open the Tagnet activation page.
7. The customer taps the keychain, enters the activation code, creates a Tagnet account (or logs into an existing account), and the tag is linked to that account. Future taps redirect to the customer's public profile.

## How to write the NFC URL to a chip (initial stock setup)

- For a small test batch, use an Android phone and an NFC writing app such as NFC Tools **yourself** to write the unique Tagnet URL once per chip.
- Choose **Write → Add a record → URL/URI**, paste the exact URL shown in admin, then write it to the tag.
- Customer does not need that app; only you need a writer app for initial stock programming.
- For larger batches, use a USB NFC writer and batch-writing workflow, or ask the tag supplier to pre-program each chip with a supplied URL list. Keep each tag URL unique.
- Do not lock the chip during tests. Test read/scan, activation, and redirect before scaling.

## Activation and ownership notes

- The tag's URL is permanent; customer profile data is stored on Tagnet, not on the chip.
- The separate one-time activation code is hashed in the database and invalidated after claim.
- The first person with the physical keychain and its package activation code can claim the tag. If an unactivated package is lost or stolen, deactivate that tag in admin before reuse.
- This is an MVP, not a hardened production system. Add rate limiting, CSRF protection, email verification/account recovery, audit logs, secure persistent sessions, database backups, and tag-transfer/support workflows before accepting real volume.

## Dark mode

Use the Dark mode / Light mode toggle in the top navigation. The preference is saved in the browser.

## Hosting caveat

The app currently uses a JSON file database, local uploads, and in-memory Express sessions. On free ephemeral hosting, database records, uploaded images, and sessions can disappear after restart/redeploy. Do not rely on this for real customers until you migrate to a managed database and persistent storage/session store. Set `NODE_ENV=production`, a unique `SESSION_SECRET`, `ADMIN_EMAIL`, and an `ADMIN_PASSWORD` of at least 16 characters for a fresh production database.

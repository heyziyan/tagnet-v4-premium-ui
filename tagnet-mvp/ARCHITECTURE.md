# Tagnet MVP — developer map

This project intentionally stays beginner-friendly: Node.js + Express, server-rendered HTML templates, one CSS file, and a local JSON database. It does **not** use React/Next.js yet.

## Where to edit things

- `server.js` — routes, form validation, database actions, and HTML templates.
- `public/style.css` — all layout, responsive styles, dark mode, animations, product cards, and profile design themes.
- `public/uploads/` — uploaded profile/product images.
- `data/db.json` — local database created automatically on first run. **Do not upload or share this file**; it contains customer contact/address information.
- `package.json` — project dependencies and run scripts.

## Main routes

- `/` — brand homepage
- `/signup`, `/login`, `/logout` — account flows
- `/dashboard` and `POST /dashboard/profile` — customer profile editor
- `/u/:username` — public profile; each page load increments the simple view counter
- `/u/:username/qr.png` — profile QR
- `/t/:code` and `POST /t/:code/activate` — NFC tag activation
- `/products` — catalog
- `/order` and `POST /order` — order request form (not payment checkout)
- `/admin` — customer/order/product/tag management and profile view analytics
- `POST /admin/users/delete` — delete a customer and linked profile/tags
- `POST /admin/products` and `POST /admin/products/delete` — product create/delete
- `POST /admin/orders/status` — update order status

## Adding a new profile category

1. In `server.js`, add an entry to `PROFILE_CATEGORIES`, for example `['law','Law Student']`.
2. In `public/style.css`, add `.category-law{...}` with the palette variables `--profile-bg`, `--profile-glow`, `--profile-accent`, `--profile-ink`, `--profile-card`, and `--profile-line`.
3. Restart with `npm start`; the new category appears in the profile editor and admin analytics filter.

## Adding a new profile style

1. Add `['style-id','Readable name']` to `PROFILE_STYLES` in `server.js`.
2. Add `.style-style-id{...}` or `.public-wrap.style-style-id ...` selectors in `public/style.css`.
3. Keep CSS scoped to `.public-wrap` so a profile style does not accidentally restyle the admin or dashboard.

## Adding a feature

1. Find the route in `server.js` that owns the feature.
2. Validate all `req.body` values on the server; never trust browser-only validation.
3. If data must persist, add a default array/field near the database migration at the top, update it, and call `save()`.
4. Add a form or table in the relevant HTML template.
5. Add CSS to `public/style.css` and scope it with a feature-specific class.
6. Run `node --check server.js`, then test the full flow in browser.

## Important MVP limits

- Orders are **requests only**. No payment gateway, shipping integration, or automated email/SMS is connected.
- Profile analytics count page loads, not unique people; refreshes and the owner's own visits count too.
- JSON storage and the in-memory session store are suitable for local testing only, not a high-volume production store.
- Before accepting real customer data, add a persistent database, CSRF protection, rate limits, email verification/recovery, backups, privacy policy, and secure deployment configuration.

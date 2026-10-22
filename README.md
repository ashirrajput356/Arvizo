# Arvizo — Watches & Earbuds

A responsive Pakistani watch and audio storefront built with React/Vite and a PHP JSON API. Orders are guest checkout and Cash on Delivery only. Product prices and stock are checked on the server inside a database transaction. SQLite is used locally by default; MySQL can be selected for deployment.

## Requirements

- Node.js 20 or newer
- PHP 8.1 or newer with PDO SQLite for local development, or PDO MySQL for deployment
- npm (on Windows, `npm.cmd` also works if PowerShell script execution blocks `npm`)

## Local setup

1. Install PHP and enable the `pdo_sqlite` and `fileinfo` extensions. Confirm `php -m` lists them.
2. If `api/.env` does not already exist, copy `api/.env.example` to `api/.env`; set a unique `ADMIN_PASSWORD` of at least 12 characters. If it already exists, keep its current admin login unless you intend to change it. Keep the real `.env` private.
3. Install frontend dependencies with `npm install`.
4. Run `npm run dev`. Vite listens on all network interfaces at port 5173 and the PHP API listens at port 8000. The first API request initializes the database, creates example products/categories, and creates the configured owner account.
5. Open `http://localhost:5173`. To use a phone on the same Wi-Fi, open `http://<computer-LAN-IP>:5173` and allow Node/PHP through the computer's firewall if prompted. Vite proxies `/api` and `/uploads` to PHP.
6. The store-owner panel is at `/admin` (also linked in the site footer). Sign in with `ADMIN_EMAIL` and `ADMIN_PASSWORD` from the private `api/.env` file. For this store, the owner email is `arvizostore@gmail.com`; an existing setup keeps its current password. The panel manages orders, products, categories, stock, prices, descriptions, and uploaded product photos. There is no public registration.

The storefront contact number is set to 0336 8915598. New order details are emailed to `STORE_ORDER_EMAIL` (default `arvizostore@gmail.com`) using PHP `mail()`. Set `STORE_EMAIL_FROM` to an address on your hosting domain and make sure the PHP host has mail delivery configured; local Windows PHP normally needs a mail server configuration. Checkout also provides a WhatsApp link with the order details prefilled. The customer must press Send in WhatsApp; automatic server-to-WhatsApp delivery requires a separately configured WhatsApp Business Cloud API integration.

## Database and deployment

Local development stores data in `api/data/arvizo.sqlite` (created automatically). The API seeds sample watches and earbuds on first start and adds the watch/audio catalog to existing databases without deleting existing products or orders. To use MySQL, create a UTF-8 database and set `DB_DRIVER=mysql`, `DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USER`, and `DB_PASSWORD` in the server-only `api/.env`. Back up the database and `api/uploads/` directory together.

For a production build, run `npm run build`. The frontend and PHP API are packaged in `dist/`; deploy that output and configure the production web server to route `/api/*` to `dist/api/index.php`, serve `/uploads/*` from `dist/api/uploads/`, and serve the frontend's `index.html` for client-side routes such as `/admin`. Set private database credentials on the PHP host, restrict write access to the SQLite file/uploads directory, and use HTTPS. Never copy `api/.env` or the local database into the public frontend build.

## Store features

- Browse, search, filter, and sort watches, earbuds, and the owner-managed catalog; responsive product details and related items.
- Persistent local shopping bag and mobile-friendly guest checkout.
- Flat Rs. 300 nationwide delivery, Cash on Delivery, server-calculated totals, transactional stock updates, and order confirmation.
- Owner dashboard at `/admin`, order search/status management/deletion, monthly reports, product/category management, and JPEG/PNG/WebP uploads up to 5 MB.
- Order notifications by configured server email, with a WhatsApp handoff link on the order confirmation.
- Database-backed newsletter signups and local fallback imagery.

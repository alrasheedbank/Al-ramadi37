# Rasheed Bank – Website & Admin Panel (AR / EN)

Bilingual (Arabic RTL / English LTR) website for Rasheed Bank, Ramadi, with a full admin panel.
**No dependencies** – only Node.js 18+ (no `npm install` needed).

```
npm start
# site   → http://localhost:3000
# panel  → http://localhost:3000/admin/
```

On the first start an admin account is created and the **generated password is printed in the console**
(only a hash is stored in `data/auth.json`). To choose your own:

```
ADMIN_USER=admin ADMIN_PASSWORD='a-long-password' npm start
```

You can change the password later in the panel (**Account**).

## What the admin panel manages
| Section | What you can edit |
|---|---|
| Services | add / edit / delete / reorder / hide; name, short text, long text, photo – each in Arabic **and** English |
| Site & Home slider | logo, bank name, tagline, footer, slider photos + titles, services heading |
| About page | texts, vision, mission, photo, highlight numbers (`{years}` and `{services}` are automatic) |
| Contact & Location | address, phone, email, hours, Google Maps link and embed |
| Media library | upload (auto-resized in the browser), copy link, delete unused images |

Detailed service text: blank line = new paragraph, `- item` = bullet list, `## Title` = sub-heading.

## Project layout
```
server.js            zero-dependency Node server + JSON API
data/content.json    all site content (edited by the panel)
public/              the website   (index.html, css/, js/)
public/admin/        the admin panel
public/uploads/      photos (logo, building, service images, your uploads)
scripts/build-preview.js   builds preview/site.html + preview/admin.html (standalone, no server)
```

## Deploying (important)
The admin panel needs a running server, so **GitHub Pages alone cannot host it** (Pages only serves static files).
Push this repo to GitHub, then deploy it on any Node host – Render, Railway, Fly.io, a VPS, etc.:

* Start command: `npm start` · Node 18+ · set `NODE_ENV=production` and `ADMIN_PASSWORD`.
* The panel saves into `data/` and `public/uploads/`. On hosts with a temporary disk these are wiped on every deploy –
  attach a persistent disk and point to it with `DATA_DIR=/data` and `UPLOADS_DIR=/data/uploads`
  (copy `data/content.json` and the seed images there on first run; the server copies `content.json` automatically).
* Put the site behind HTTPS (cookies become `Secure` automatically in production).

## Security notes
* Passwords are hashed with scrypt; sessions are HttpOnly + SameSite=Strict cookies; login is rate-limited (5 failures / 15 min).
* State-changing requests need the `X-Requested-With` header and a same-origin `Origin` (CSRF).
* Uploads accept only real PNG / JPEG / WEBP / GIF files (magic bytes checked), max 6 MB.
* All content is rendered as text (no HTML injection); a strict Content-Security-Policy is sent.

## Language & texts
Fixed interface words (menu, buttons such as “Learn more”) are in `public/js/site.js` (`UI` object).
Everything else is editable from the panel.


## Yönetim paneli – sayfalar / Admin pages
Dashboard · Services (category tag, gallery) · **Home slider** (shape: arch, pointed arch, rounded, circle, oval, capsule, leaf, hexagon, rectangle; layout split/full; size, proportion, effect, speed, buttons, live preview) · **Appearance** (colour themes, colours, corner radius, opening animation) · Brand & footer (logo, SEO, social links) · **Sections & menu** (order, show/hide, menu names) · About · Contact & map · **Texts & buttons** (every fixed word) · Media · Account.

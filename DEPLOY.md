# Deploy Guide — Football Community Page

Two files. `index.html` is the entire site. This file is instructions only — do not upload it if you don't want to.

---

## 1. Edit the content (no coding needed)

Open `index.html` in any text editor. Scroll to the bottom and find:

```
const CONFIG = {
```

Everything on the page comes from that one block. Change the text between the quote marks. Do not delete commas or brackets.

| What you change | Field |
|---|---|
| Channel name in the big headline | `brand` |
| Small orange label above it | `eyebrow` |
| One-line description | `tagline` |
| **Your Telegram invite link** | `telegram` |
| Button text | `ctaLabel` |
| Hero image filename | `image` |
| Upcoming matches + countdown target | `fixtures` |
| The three "what you get" blocks | `gets` |
| Ground rules list | `rules` |
| Legal disclaimer | `disclaimer` |

**Test before uploading:** double-click `index.html` — it opens in your browser and works fully offline. If something breaks, press F12 and read the red error in the Console. A missing comma is the usual cause.

---

## 2. The hero image

1. Save your image as `hero.jpg` in the same folder as `index.html`.
2. Set `image: "hero.jpg"` in CONFIG.
3. Leave it as `image: ""` and the page shows a styled pitch-pattern placeholder instead. Nothing breaks.

**Use a photo you own or have licensed.** Press photos of players from Getty, AP, Reuters or AFP are copyrighted — using one on a live commercial page is a real infringement risk, not a theoretical one. Free commercial-use options: Unsplash, Pexels, Pixabay. Better still: your own stadium or matchday photos, or an AI-generated crowd/pitch image.

---

## 3. The countdown — read this

The countdown does **not** reset when someone refreshes. It targets a real kickoff time from your `fixtures` list, rolls to the next match automatically, and switches to a pulsing **LIVE NOW** state during the match window.

That means you must keep `fixtures` updated, or the board eventually falls back to "Next fixtures posted in the channel." Budget five minutes a week.

Time format is `"YYYY-MM-DDTHH:MM"` on a 24-hour clock, in your own timezone:

```
{ home:"Arsenal", away:"Liverpool", comp:"Premier League", kickoff:"2026-08-29T21:00" }
```

---

## 4. Go live on GitHub Pages (free, ~10 minutes)

1. Create a free account at github.com.
2. Click **+** → **New repository**. Name it (e.g. `matchday-room`). Set it **Public**. Create.
3. On the repo page click **uploading an existing file**. Drag in `index.html` (and `hero.jpg` if you have one). Click **Commit changes**.
4. Go to **Settings** → **Pages** (left sidebar).
5. Under *Build and deployment* → *Source*, choose **Deploy from a branch**. Branch: **main**, folder: **/ (root)**. Save.
6. Wait 1–3 minutes, then refresh. Your live URL appears at the top:
   `https://YOURUSERNAME.github.io/matchday-room/`

**To edit later:** open `index.html` in the repo, click the pencil icon, edit the CONFIG block right in the browser, and commit. Live in about a minute. You can do this from your phone.

---

## 5. Add your own domain

Buy the domain first (GoDaddy, Namecheap, Hostinger, Cloudflare — around ₹700–1,200/year for a `.com`).

### Step A — tell GitHub

Repo → **Settings** → **Pages** → **Custom domain** → type `yourdomain.com` → **Save**.

Do this *before* the DNS step. Adding DNS first without claiming the domain in GitHub leaves a window where someone else can host on it.

### Step B — add DNS records at your registrar

For the root domain (`yourdomain.com`) — four **A** records, all with host `@`:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

Optionally add four **AAAA** records (IPv6), also host `@`:

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

For `www` — one **CNAME** record:

```
Host:  www
Value: YOURUSERNAME.github.io
```

Point the CNAME at `YOURUSERNAME.github.io` only. Never include the repo name.

### Step C — force HTTPS

Wait for DNS to propagate (usually 15 minutes, up to 24 hours). Then go back to **Settings → Pages** and tick **Enforce HTTPS**. GitHub issues a free SSL certificate.

If the checkbox is greyed out, DNS hasn't propagated yet. Check at dnschecker.org and come back later.

### Note if you use Cloudflare

Set the records to **DNS only** (grey cloud), not Proxied (orange cloud), until HTTPS is working. Proxying before the certificate is issued is the single most common cause of the "certificate not yet created" error.

---

## 6. Before you send traffic

- [ ] Telegram link opens the correct channel — test on a phone, not just desktop
- [ ] Fixtures are real and in the future
- [ ] Hero image is one you own or have licensed
- [ ] Disclaimer text matches what the channel actually does
- [ ] Page loads under 3 seconds on 4G
- [ ] Opened it on a real phone, not just a browser resize

---

## 7. Measurement

The page has no tracking. To measure joins, paste this just above `</body>` and replace the ID:

```html
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXX');
  document.querySelectorAll('.cta').forEach(function(b){
    b.addEventListener('click', function(){ gtag('event','join_click'); });
  });
</script>
```

The metric that matters is **cost per Telegram join**, then **7-day retention inside the channel**. A cheap join that leaves in 48 hours is a wasted click.

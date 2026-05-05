# PolymerIQ — Materials Intelligence Platform

A commercial polymer materials database with AI assistant, live manufacturer catalog search, education resources, and research articles.

## Live Demo

After deployment your URL will be: `https://YOUR_USERNAME.github.io/polymeriq`

---

## Deployment Guide (GitHub Pages — Free)

### Step 1 — Create GitHub account
Go to [github.com](https://github.com) and sign up (free).

### Step 2 — Create a new repository
1. Click **+** (top right) → **New repository**
2. Name it `polymeriq`
3. Set to **Public**
4. Check **Add a README file**
5. Click **Create repository**

### Step 3 — Upload the file
1. In your new repository, click **Add file** → **Upload files**
2. Drag and drop `index.html` from this folder
3. Scroll down, click **Commit changes**

### Step 4 — Enable GitHub Pages
1. Go to your repository **Settings** tab
2. Left sidebar → **Pages**
3. Under **Source**, select **Deploy from a branch**
4. Branch: **main** · Folder: **/ (root)**
5. Click **Save**
6. Wait 2–3 minutes, then visit: `https://YOUR_USERNAME.github.io/polymeriq`

### Step 5 — Add your Anthropic API key
1. Get your API key from [console.anthropic.com](https://console.anthropic.com) (free tier available)
2. In your repository, click `index.html` → **Edit (pencil icon)**
3. Find this line:
   ```js
   window.ANTHROPIC_API_KEY = 'YOUR_ANTHROPIC_API_KEY_HERE';
   ```
4. Replace `YOUR_ANTHROPIC_API_KEY_HERE` with your actual key
5. Click **Commit changes**

That's it — your app is live!

---

## Security Note

**For personal / internal use**, putting the API key directly in the HTML file is fine.

**For public-facing deployment** (where strangers could use your key), set up a simple backend proxy:

### Option A — Cloudflare Worker (free, 5 minutes)
1. Go to [workers.cloudflare.com](https://workers.cloudflare.com)
2. Create a new Worker with this code:
```js
export default {
  async fetch(request, env) {
    if (request.method !== 'POST') return new Response('Method not allowed', { status: 405 });
    const body = await request.json();
    const response = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'x-api-key': env.ANTHROPIC_API_KEY,
        'anthropic-version': '2023-06-01',
      },
      body: JSON.stringify(body),
    });
    const data = await response.json();
    return new Response(JSON.stringify(data), {
      headers: { 'Content-Type': 'application/json', 'Access-Control-Allow-Origin': '*' }
    });
  }
};
```
3. Add `ANTHROPIC_API_KEY` as an Environment Variable in the Worker settings
4. In `index.html`, change the fetch URL from `https://api.anthropic.com/v1/messages` to your Worker URL

### Option B — Vercel (free)
Deploy as a Next.js app with an API route that proxies requests. Ask ChatGPT or Claude to scaffold this for you.

---

## Features

| Feature | Description |
|---|---|
| **Material Database** | 100+ seed grades, expandable to thousands via Live Fetch |
| **Live Fetch** | Search manufacturer catalogs in real time (Covestro, Envalior, Toray, LANXESS, etc.) |
| **Compare** | Side-by-side comparison of up to 5 materials with property highlighting |
| **CSV Export** | Export filtered results or comparison table |
| **AI Assistant** | Natural language search with live web access |
| **Education** | 18 curated textbooks, courses, journals, and databases |
| **Research** | 15 curated papers + live journal search |

## Expanding the Database

In the Database tab, use the **Live Fetch** bar to add grades from any manufacturer:

```
"Covestro Makrolon PC grades"
"Envalior Akulon PA6 GF"
"Toray Torelina PPS"
"LANXESS Durethan PA6 PA66"
"Solvay Torlon PAI"
"DSM Stanyl PA46"
"Celanese Vectra LCP"
"Sabic Ultem PEI"
"Solvay Solef PVDF"
"Daikin Neoflon ETFE"
```

Each search adds 10–20 verified grades. The database grows with each session.

## Included Manufacturers (Seed Data)

Sabic · BASF · Celenese · Syensqo · Arkema · Lotte Chemical · LG Chemical · Evonik · Victrex · EMS Grivory · Polyplastics · Mitsubishi · Teijin · Idemitsu · Samyangsa

## License

MIT — free to use, modify, and distribute.

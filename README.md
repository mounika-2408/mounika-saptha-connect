# SapthaConnect

A lightweight, blazing-fast web application built with pure HTML, CSS, and JavaScript, powered by Hono on Cloudflare Workers.

[cloudflarebutton]

## Description

SapthaConnect is a minimal, edge-native starter project that combines vanilla frontend simplicity with the power of Cloudflare Workers. It uses Hono as a fast, lightweight backend framework to serve static assets from the edge with zero cold starts, global distribution, and instant scalability.

This project is ideal for landing pages, prototypes, micro-frontends, and learning edge development without the overhead of heavy frameworks.

## Key Features

- **Zero-Framework Frontend** — Pure HTML, CSS, and JavaScript for maximum performance and simplicity
- **Edge Backend with Hono** — Ultra-fast routing and static file serving via Hono on Cloudflare Workers
- **Global Deployment** — Runs on Cloudflare's global network with low latency worldwide
- **Static Asset Serving** — Optimized SPA handling with `single-page-application` fallback
- **Modern Styling** — Responsive, gradient-based UI with glassmorphism card design
- **Instant Interactivity** — Vanilla JS counter example to get started quickly
- **Type-Safe Tooling** — TypeScript, ESLint, and Cloudflare Workers Types preconfigured

## Technology Stack

- **Frontend:** HTML5, CSS3, Vanilla JavaScript (ES Modules)
- **Backend:** [Hono](https://hono.dev/) `^4.9.8`
- **Runtime / Platform:** [Cloudflare Workers](https://workers.cloudflare.com/)
- **Static Assets:** Cloudflare Workers Assets (`./public`)
- **Dev Tools:**
  - [Wrangler](https://developers.cloudflare.com/workers/wrangler/) `^4.39.0` for local dev and deployment
  - [Bun](https://bun.sh/) for package management and scripts
  - TypeScript `^5`, ESLint `^9`, `@cloudflare/workers-types` `^4`
- **Node Compatibility:** Node.js `>=18`

## Project Structure

```text
├── public/
│   ├── index.html      # App entry point
│   ├── styles.css      # Global styles
│   └── app.js          # Client-side JavaScript
├── worker/
│   └── index.js        # Hono app + Workers entry point
├── scripts/
│   └── dev.ts          # Local dev bootstrap script
├── wrangler.jsonc      # Cloudflare Workers configuration
├── package.json        # Dependencies and scripts
└── README.md
```

## Prerequisites

- [Bun](https://bun.sh/docs/installation) v1.0 or later
- [Wrangler CLI](https://developers.cloudflare.com/workers/wrangler/install-and-update/) (installed as devDependency, can also be used via `bunx`)
- A [Cloudflare account](https://dash.cloudflare.com/sign-up) for deployment

## Setup / Installation

1. Clone the repository:

   ```bash
   git clone <your-repo-url>
   cd sapthaconnect-6-inulkj9wrk9vrf7yppf
   ```

2. Install dependencies with Bun:

   ```bash
   bun install
   ```

3. Verify Wrangler authentication (only needed for deployment):

   ```bash
   bunx wrangler login
   ```

## Usage Examples

### Local Development

Start the local dev server with hot reload:

```bash
bun run dev
```

This runs `bun scripts/dev.ts`, which boots Wrangler dev with Workers Assets from `./public`. Open the URL shown in the terminal (typically `http://localhost:8787`).

### Editing the Frontend

- Edit `public/index.html` to change structure and content.
- Edit `public/styles.css` to update styling.
- Edit `public/app.js` to add client-side interactivity. The file is loaded as an ES module:

```html
<script src="/app.js" type="module"></script>
```

Example — counter interaction in `public/app.js`:

```js
let count = 0;
const button = document.getElementById('counter');
const countSpan = document.getElementById('count');

button.addEventListener('click', () => {
  count++;
  countSpan.textContent = count;
});
```

### Editing the Backend

The Worker entry is `worker/index.js`:

```js
import { Hono } from 'hono'
import { serveStatic } from 'hono/cloudflare-workers'

const app = new Hono()

// Serve static files from public/
app.use('/*', serveStatic({ root: './public' }))

export default app
```

Add API routes before the static handler, for example:

```js
app.get('/api/hello', (c) => {
  return c.json({ message: 'Hello from the edge!' })
})
```

## Development Instructions

Available scripts from `package.json`:

| Command | Description |
|---|---|
| `bun run dev` | Start local dev server via `scripts/dev.ts` |
| `bun run build` | Build / validate with Wrangler (`wrangler build`) |
| `bun run deploy` | Deploy to Cloudflare Workers |
| `bun run lint` | Run ESLint |

Lint the project:

```bash
bun run lint
```

Key config notes:

- `wrangler.jsonc` sets `main` to `worker/index.js` and serves `directory: ./public` as Workers Assets.
- `not_found_handling: single-page-application` returns `index.html` for unknown routes, enabling SPA-style routing.
- `compatibility_date: 2025-04-24` pins Workers runtime APIs.

## Deployment

### Deploy to Cloudflare Workers

1. Log in to Cloudflare:

   ```bash
   bunx wrangler login
   ```

2. Deploy:

   ```bash
   bun run deploy
   ```

3. Wrangler will output your live URL, e.g. `https://sapthaconnect-6-inulkj9wrk9vrf7yppf.workers.dev`.

### One-Click Deploy

You can deploy this repository directly to Cloudflare Workers with one click:

[cloudflarebutton]

> The button uses `https://deploy.workers.cloudflare.com/?url=${repositoryUrl}` and will create a copy of this repo and deploy it to your Cloudflare account.

### Custom Domain & Settings

1. Go to Cloudflare Dashboard > Workers & Pages > select your Worker.
2. Under Settings > Domains & Routes, add a custom domain or route.
3. No environment variables or KV/D1 bindings are required for the default template.

## Configuration

- **Worker name:** `sapthaconnect-6-inulkj9wrk9vrf7yppf` (change `name` in `wrangler.jsonc` to rename)
- **Assets directory:** `./public`
- **Compatibility date:** `2025-04-24`

To rename the project, update `name` in both `package.json` and `wrangler.jsonc`.

## Contributing

1. Fork the repository.
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Install dependencies: `bun install`
4. Make your changes and run lint: `bun run lint`
5. Test locally: `bun run dev`
6. Submit a pull request with a clear description.

## License

This project is provided as-is for demonstration and starter purposes. Add your preferred license (e.g., MIT) here before publishing as open source.

---

Built with vanilla JavaScript + Hono + Cloudflare Workers.

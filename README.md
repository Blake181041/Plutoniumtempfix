# Plutonium

> The unblocked games website made for YOU.

[![GitHub License](https://img.shields.io/github/license/Plutonium-Net/Plutonium)](https://github.com/Plutonium-Net/Plutonium)
[![GitHub Stars](https://img.shields.io/github/stars/Plutonium-Net/Plutonium)](https://github.com/Plutonium-Net/Plutonium/stargazers)
[![GitHub Issues](https://img.shields.io/github/issues/Plutonium-Net/Plutonium)](https://github.com/Plutonium-Net/Plutonium/issues)

Plutonium is a web-based platform for games, browsing, media, and customization, built around a familiar browser-style interface.

## Features

* **Games**: Play games directly through Plutonium.
* **Browser**: Browse the web from within Plutonium.
* **Media**: Access media through the platform.
* **Customization**: Customize themes and settings.
* **Tabs**: Use a browser-style tabbed interface.
* **Web-Based**: Runs directly in a modern web browser.

## Running Locally

Plutonium must be served through a local web server. Opening `index.html` directly with `file://` is not supported.

Clone the repository:

```bash
git clone https://github.com/Plutonium-Net/Plutonium.git
cd Plutonium
```

Start a local web server. For example, with Python:

```bash
python3 -m http.server 8080
```

Then open:

```text
http://localhost:8080
```

## Vercel and Cloudflare Pages

VanilliaPXY must use the included server-side relay. Its upstream responses can
contain `X-Frame-Options: SAMEORIGIN` and CSP restrictions from target websites;
loading that remote host directly in an iframe makes those sites appear blocked.
The frontend now loads `/vanillia-embed/<server>/vanillia?url=...` instead.
The relay removes upstream framing/CSP restrictions and rewrites proxy links,
assets, redirects and runtime URLs to stay on the local relay. Proxied pages run
in an opaque iframe sandbox so target scripts cannot read the app's storage or
account data. Target service-worker registration is disabled in that sandbox;
the worker endpoint is additionally scope-limited to `/vanillia-embed/<server>/`.
Only the three configured Vanillia hosts and known proxy routes are relayed;
Plutonium cookies and authorization headers are never forwarded.

- **Vercel:** import this directory as the project root, choose **Other** (no
  framework), and leave the build command and output directory unset. The included
  `vercel.json` routes relay traffic to the Node.js function in `api/vanillia.mjs`.
  Use Node.js 22 or newer. No new environment variables or dependencies are needed.
- **Cloudflare Pages:** use no framework, no build command (or `exit 0`), and
  **`.`** as the output directory. Deploy with Git integration or Wrangler so the
  advanced-mode `_worker.js` and its imports are bundled as a Pages Function.
  `_routes.json` invokes it only for `/vanillia-embed/*`; all other files remain
  static. Dashboard drag-and-drop without Functions is not sufficient.

A plain static server such as Python's server above still serves the app, but
cannot run this relay. For a function-capable local preview, run either:

```bash
npx vercel dev --local --yes
# or
npx wrangler pages dev .
```

Do not add an SPA catch-all rewrite for relay paths, or a hosting-level policy
that disallows same-origin frames. Proxied text responses are bounded to 8 MiB
and upstream requests time out after 25 seconds. Binary assets stream unchanged.

This fixes iframe policy blocking, **not upstream anti-bot restrictions**.
DuckDuckGo may still return its error/challenge page to the Vanillia server's IP.
WebSocket upgrades are not supported by this HTTP relay on Vercel. Sites that
require their own service workers or persistent first-party storage may have
limited functionality in the sandbox.

Run the dependency-free regression checks with:

```bash
node --test tests/vanillia-relay.test.mjs
```

## Contributing

Contributions, improvements, and bug reports are welcome.

Please test changes locally before submitting a pull request.

## License

See the [LICENSE](LICENSE) file for licensing information.

---

**Plutonium Network**

[GitHub](https://github.com/Plutonium-Net) · [Repository](https://github.com/Plutonium-Net/Plutonium)

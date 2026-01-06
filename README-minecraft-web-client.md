# Zardoy Minecraft Web Client (link + hosting instructions)

This repository will not include the project's source. Instead this README links to the official upstream project and provides clear instructions to run and host the web client on a custom domain.

Upstream project

- Repository: https://github.com/zardoy/minecraft-web-client
- CodeSandbox (live demo / example): https://codesandbox.io/p/github/zardoy/minecraft-web-client

What this file does

- Provides a pointer to the upstream project.
- Gives quick steps to run locally and to deploy to a domain (static hosting + optional proxy for full server connectivity).

Quick local run (snapshot / development)

1. Clone upstream (do not add code to this repo):

   git clone https://github.com/zardoy/minecraft-web-client.git
   cd minecraft-web-client

2. Install dependencies and build (follow upstream README for exact commands — examples below are typical for JS projects):

   npm install
   npm run build

3. Serve the built static files locally to verify (examples):

   npx serve dist    # or `npx serve build` depending on the project's output folder

4. Open the served URL in your browser and test.

Hosting options and domain setup

Option A — Static-only deployment (quickest, if you only need the web UI):

- Build the project and host the resulting static files on any static hosting provider that supports custom domains: GitHub Pages, Netlify, Vercel, Cloudflare Pages, S3 + CloudFront.
- Deploy steps (example with Vercel):
  - Import the upstream repo into Vercel.
  - Set the build command to `npm run build` and the output directory to the project's build folder (commonly `dist` or `build`) — check upstream README.
  - Add your custom domain in the Vercel dashboard and follow DNS instructions.

Note: Static hosting is fine to serve the client UI, but if you want to connect to remote Minecraft servers from the browser you will probably need a WebSocket->TCP proxy (see Option B).

Option B — Full functionality (client + proxy for connecting to servers)

- The browser-based client typically needs a WebSocket-based proxy that accepts the client's WebSocket connections and forwards them to a Minecraft server (TCP). The upstream repo documents recommended proxy components and how to configure them.
- Typical deployment pattern:
  1. Deploy the static client to a domain or subdomain (e.g., client.example.com) using Vercel/Netlify/Cloudflare Pages.
  2. Deploy the proxy service to a host that supports long-lived WebSocket connections and TCP forwarding (examples: Render, Railway, DigitalOcean App Platform, a VPS on DigitalOcean/Hetzner, or a container on Render). Use a separate subdomain for the proxy (e.g., proxy.example.com).
  3. Configure TLS (HTTPS/WSS) for both client and proxy domains. Most providers provide automatic certificates.
  4. Configure the client build or runtime to point to your proxy WebSocket URL (e.g., `wss://proxy.example.com`) — check the upstream config options.

Recommended minimal providers:
- Static site: Vercel / Netlify / Cloudflare Pages (easy custom domain setup)
- Proxy service: Render / Railway / a small VPS (make sure it supports WebSockets and TCP forwarding)

DNS / Custom domain notes

- Add the domain to your hosting provider (Vercel/Netlify/Pages) and follow their DNS verification steps.
- For a single-host static+proxy setup, use subdomains: `client.example.com` -> static host; `proxy.example.com` -> proxy host.
- If you prefer a single origin, place a reverse proxy that routes `/` to the client files and routes WebSocket requests to the proxy backend. A reverse proxy (NGINX, Caddy) must be configured to support WebSocket forwarding.

Security & CORS

- Make sure the proxy and client are configured to allow the client's origin (CORS) and to use secure WebSockets (wss://) in production.
- If authentication is required, follow upstream guidance to wire any auth tokens or sessions through the proxy.

What I will add to this repository now (defaults chosen)

- File added: `README-minecraft-web-client.md` at the repository root.
- Branch: `main`
- Commit message: "Add README with link and hosting instructions for zardoy/minecraft-web-client"

If you'd like I can instead:
- Create a short GitHub Pages site (adds a small workflow or gh-pages branch) that points to the upstream build artifacts.
- Add a CNAME file and example DNS records for your actual domain (you must provide the domain name).
- Create a small script that automates building and deploying a cloned upstream repo to a provider.

Please confirm if you want any of those extras, or provide your domain now if you want me to add a CNAME and example DNS guidance.

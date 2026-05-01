# Robrary AI Games Library

A collection of games built using modern AI codegen tools.

## Overview

This project serves as a hub for various browser-based games. The visual aesthetic is a "greyscale neon" terminal style.

## Games Collection

The library currently includes links to the following games:

*   **Snake**
*   **Missile Defense**
*   **Defender**
*   **Space Invader**

## Deployment

This site is set up to be hosted on GitHub Pages using the `/docs` folder.

## Adding a New Game

Each game in this library is hosted as a separate GitHub Pages site and accessed via a subdomain of `robrary.com`. To add a new game:

### 1. Create the Game Repo

Create a new repo under the `therobrary` GitHub organization. The game content should live in a `docs/` directory (served via GitHub Pages).

### 2. Configure GitHub Pages

In the repo settings → Pages:
- Set **Source** to "Deploy from a branch"
- Select branch: `main`, folder: `/docs`
- Enable "Enforce HTTPS"

### 3. Add a CNAME File

Create a file `docs/CNAME` in the repo with the custom subdomain:

```
<game-name>.robrary.com
```

For example, `snake.robrary.com` for the Snake game.

### 4. Add Cloudflare DNS Record

In the Cloudflare dashboard for `robrary.com`, add a CNAME record:

| Field | Value |
|---|---|
| **Name** | `<game-name>` |
| **Type** | `CNAME` |
| **Target** | `therobrary.github.io` |
| **Proxy** | Proxied (orange cloud) |

### 5. Update the Games Menu

In `docs/index.html`, add a new `<a class="game-card">` entry with the URL `https://<game-name>.robrary.com`.

### How It Works

- **DNS**: Each game subdomain CNAMEs to `therobrary.github.io` via Cloudflare
- **GitHub Pages**: The `CNAME` file in each repo's `docs/` directory tells GitHub which custom domain to serve
- **SSL**: GitHub automatically provisions SSL certificates for the custom domains
- **No Worker**: This setup uses DNS-only routing — no Cloudflare Worker is needed

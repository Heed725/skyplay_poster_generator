# Skyplay Poster Generator

A responsive playlist poster editor inspired by the layout of Hybridbox, with an optional Spotify OAuth backend.

## Live frontend

https://heed725.github.io/skyplay_poster_generator/

The GitHub Pages website is static. The Node.js API requires **separate hosting** (e.g. Render). GitHub Pages cannot run Node.js.

## Features

- Create posters with a live canvas preview, custom layouts, colors and text.
- Import an owned/collaborative Spotify playlist after signing in with Spotify.
- Fetch playlist title, creator, cover image and up to 100 tracks.
- Export PNG and JPG; use Print to save as PDF.
- Save editable projects locally as JSON.
- No server-side Spotify credentials are exposed in the browser.

## Deploy the Spotify backend on Render

1. Create a Spotify developer app at https://developer.spotify.com/dashboard and obtain the **Client ID** and **Client Secret**. Spotify development mode requires Premium and imposes user restrictions.
2. Open https://dashboard.render.com/ and create a **New Web Service** linked to `Heed725/skyplay_poster_generator`. Use Node runtime, `npm install` build command, and `npm start` start command. Alternatively deploy using `render.yaml`.
3. Once Render gives you a hostname (example: `https://YOUR-SERVICE.onrender.com`), configure these **secret environment variables in Render**, not GitHub:
   - `SPOTIFY_CLIENT_ID` = your Spotify Client ID
   - `SPOTIFY_CLIENT_SECRET` = your Spotify Client Secret
   - `PUBLIC_BASE_URL` = your actual Render service HTTPS URL, **without trailing slash**
   - `FRONTEND_URL` = `https://heed725.github.io/skyplay_poster_generator/`
4. In Spotify Developer Dashboard > Your app > Settings, set the **Redirect URI** to `https://YOUR-SERVICE.onrender.com/auth/callback`. It must exactly match `PUBLIC_BASE_URL + /auth/callback`.
5. Deploy/redeploy. Verify `https://YOUR-SERVICE.onrender.com/health` returns `{"ok":true,...,"spotifyConfigured":true}`.
6. Open Skyplay on GitHub Pages. In the **Create | Order** tab, paste your Render URL into **Skyplay API server URL** and click **Connect Spotify**. Authenticate with an authorized Spotify account, then paste your Spotify playlist URL and click **Import**.

**Important Spotify restrictions (2026):** Spotify's Development Mode requires Premium and is limited to up to five authorized users. The `GET /playlists/{id}/items` API returns tracks only for playlists the authenticated user owns or collaborates on. An arbitrary public playlist may return 403. A playlist title and artwork may be available even when the tracks are not. Skyplay leaves manual editing available for unsupported playlists.

**Security:** Do not share your Spotify Client Secret, upload `.env` files, or paste credentials into the frontend. Login sessions are stored in server memory, expire after inactivity, and are lost on restart. This is suitable for small hobby deployments rather than production scale. Enable a persistent session store, rate limiting, and monitoring before large public use.

## Run locally

Requires Node.js 20+.

```bash
npm install
# Export environment variables from .env.example with your own values
npm start
```

Open http://127.0.0.1:3000/ and use http://127.0.0.1:3000/auth/callback as your registered redirect URI for local testing (Spotify may require an HTTPS or permitted loopback redirect based on current Spotify settings).

## API

- `GET /health` — check server status
- `GET /auth/login` — start Spotify authorization
- `GET /auth/callback` — OAuth redirect
- `GET /api/playlist?url=...` — playlist details (requires Skyplay session bearer token)
- `GET /api/artwork?url=...` — permitted Spotify artwork proxy (requires Skyplay session bearer token)

Spotify content must be used in accordance with Spotify's policies, including artwork presentation and attribution. Skyplay is independent of Spotify and Hybridbox.

## Automatic playlist import and poster styling

After connecting Spotify once, simply paste a supported Spotify playlist link into **Create | Order**. Skyplay automatically imports its metadata, track list and cover image and generates complementary colors and a suggested font from the image. If Spotify authentication is needed, connect and return to resume your previously pasted playlist. Spotify's playlist-access restrictions still apply.

Under **Layout & Size**, adjust title size, artist size, track size, track spacing, gap below artwork, title/artist spacing, letter spacing and page margin. **Auto-fit to cover & tracks** chooses a suggested composition; the settings remain editable. Imported artwork uses the Spotify metadata URL with an oEmbed fallback. The Render backend must deploy the newest `main` commit for the fallback to be available.

## Latest update: automatic cover and font controls

The existing Spotify OAuth + Render backend remains in place. The compatible website update improves cover load/decode confirmation and automatically extracts matching background, foreground and accent colors after a playlist import or manual image upload. The **Layout & Size** panel includes separate title, artist and track size controls, letter spacing, track spacing, gap below artwork and automatic layout sizing. Paste a playlist link after connecting Spotify to trigger import automatically. The older standalone ZIP's alternate API server was not used because it would break the currently deployed OAuth flow.

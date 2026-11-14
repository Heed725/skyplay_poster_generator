# Skyplay Poster Generator

A free, responsive, Hybridbox-inspired playlist poster editor. This is an independent original implementation, not affiliated with Spotify or Hybridbox.

## Live website (after enabling Pages)
https://heed725.github.io/skyplay_poster_generator/

## Features
- Poster editing and instant live preview
- Playlist title, artist, year, tracks, and cover image upload
- Four tabs: Create | Order, Layout & Size, Poster Color, Toggles/Other
- Classic portrait, A4, square and landscape formats
- Adjustable margins, artwork sizes, title scaling, track columns
- Background/text/accent color pickers and suggested palettes
- Track, year, frame, number, and footer visibility controls
- Export as PNG or JPG; Print / Save PDF using your browser
- Save and restore editable JSON projects

## Setup
No build is necessary. Open `index.html` directly in your browser.

### GitHub Pages
1. Open the repository **Settings**.
2. Select **Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**.
4. Select **main** and **/(root)**, then **Save**.
5. Once deployment finishes, open https://heed725.github.io/skyplay_poster_generator/

## Spotify playlist import
All manual editing functions work on GitHub Pages. **Automatic Spotify playlist import is not included out of the box.** Spotify API credentials must be stored securely on a separate backend (not in browser JavaScript or the repository), and that backend must implement `GET /api/playlist?url=...`. It should return JSON with `title`, `artist`, `tracks` and optional `image`.

To point the editor to your backend, in the browser developer console run:
```js
localStorage.setItem('skyplay_api_url', 'https://YOUR-BACKEND.example.com')
```
Then reload. The backend must support HTTPS and CORS from the Pages origin. Spotify API access is subject to Spotify's developer requirements.

## Rights
Use artwork and music metadata you are allowed to reproduce.
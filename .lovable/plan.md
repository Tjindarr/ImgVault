
# Publish ImgVault to Unraid Community Apps

Mirrors the setup from your **Family Hub Display** repo: GitHub Actions builds the Docker image and pushes to GHCR on every push to `main`, and an Unraid CA template XML lets users install it via "Install from URL".

## Assumptions (tell me if you want different values)
- GitHub repo: `Tjindarr/imgvault` (matches your other repo style)
- GHCR image: `ghcr.io/tjindarr/imgvault:latest`
- Default host port: `8180` → container `8080` (already in your README)
- Appdata base: `/mnt/user/appdata/imgvault`

## Files to add

**1. `.github/workflows/docker-publish.yml`**
Same workflow as Family Hub Display — builds on push to `main` and on `v*.*.*` tags, pushes multi-tag (`latest`, semver, branch) to GHCR with GHA build cache.

**2. `templates/imgvault.xml`** — Unraid Community Apps template
- Name: `ImgVault`
- Repository: `ghcr.io/tjindarr/imgvault:latest`
- Category: `MediaApp:Photos Productivity:`
- WebUI: `http://[IP]:[PORT:8080]/`
- Icon: `https://raw.githubusercontent.com/Tjindarr/imgvault/main/icon.png`
- Ports: `8180 → 8080`
- Volumes (matching your README):
  - `/mnt/user/Onedrive/Bilder` → `/data/photos` (rw)
  - `/mnt/user/appdata/imgvault/db` → `/data/db`
  - `/mnt/user/appdata/imgvault/thumbs` → `/data/thumbnails`
  - `/mnt/user/appdata/imgvault/transcoded` → `/data/transcoded`
  - `/mnt/user/appdata/imgvault/converted` → `/data/converted`
  - `/mnt/user/appdata/imgvault/trash` → `/data/trash`

**3. `icon.png`** (repo root) — copy of `public/icon-512.png` so the CA template can reference it via raw.githubusercontent.com.

**4. `LICENSE`** — MIT (matches Family Hub Display).

**5. `README.md`** — add an "Unraid (Community Applications)" section with the Install-from-URL instructions pointing to the template raw URL.

## Icon changes
- Update `index.html` favicon to point at `/icon-192.png` (the PWA icon) and remove the default `public/favicon.ico` so browsers stop loading the old one.
- The PWA already uses `icon-512.png` / `icon-192.png`, so phone home-screen icon stays the same.

## After implementation — what you do
1. Connect this Lovable project to GitHub (Plus menu → GitHub → Connect) and create repo `imgvault` under your account.
2. The Actions workflow runs automatically on the first push; image lands at `ghcr.io/tjindarr/imgvault:latest`.
3. In GHCR package settings, set the package visibility to **Public** (otherwise Unraid users can't pull it).
4. In Unraid → Apps → Install from URL, paste:
   `https://raw.githubusercontent.com/Tjindarr/imgvault/main/templates/imgvault.xml`
5. To get listed in the Community Apps store itself, submit a PR to the `Squidly271/AppFeed` (or use the Unraid forum CA submission thread) referencing the template URL above — that's a manual one-time step outside Lovable.

If the repo name, port, or appdata paths above are wrong, tell me and I'll adjust before implementing.

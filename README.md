# 6ure Files GitHub Update Server

This folder is the GitHub Pages update server.

Use this repo pattern:

```text
Repo name: 6ure-files-updates
Pages URL: https://YOUR_GITHUB_USERNAME.github.io/6ure-files-updates/
Manifest:  https://YOUR_GITHUB_USERNAME.github.io/6ure-files-updates/latest.json
```

## Why GitHub Releases for setup EXE files?

Keep `latest.json` on GitHub Pages, but upload setup EXE files to GitHub Releases. GitHub Pages has site/repo/bandwidth
limits, while GitHub's docs recommend Releases for distributing large binaries.

## First Setup

1. Create a public GitHub repo named `6ure-files-updates`.
2. Upload this folder's files to that repo:
   - `.nojekyll`
   - `index.html`
   - `latest.json`
   - `README.md`
   - `make-latest.ps1`
   - `set-app-update-config.ps1`
3. In GitHub repo settings, enable Pages:
   - Source: `Deploy from a branch`
   - Branch: `main`
   - Folder: `/root`
4. Update the app config before building your setup:

```powershell
.\set-app-update-config.ps1 -Owner "YOUR_GITHUB_USERNAME" -Repo "6ure-files-updates"
```

This writes:

```json
{
  "manifestUrl": "https://YOUR_GITHUB_USERNAME.github.io/6ure-files-updates/latest.json",
  "channel": "stable",
  "allowInsecure": false
}
```

## Publishing A New Version

1. Build your setup EXE.
2. Create a GitHub Release with tag `v1.0.1`.
3. Upload the setup EXE as a release asset.
4. Generate a new `latest.json`:

```powershell
.\make-latest.ps1 `
  -Owner "YOUR_GITHUB_USERNAME" `
  -Repo "6ure-files-updates" `
  -Version "1.0.1" `
  -PackagePath "C:\path\to\6ure-files-setup-1.0.1.exe" `
  -Notes "Added updater system"
```

5. Commit and push the updated `latest.json`.

Installed apps will read the new version from:

```text
https://YOUR_GITHUB_USERNAME.github.io/6ure-files-updates/latest.json
```

## Installer Args

Default installer args are:

```text
/VERYSILENT /NORESTART
```

Use this for Inno Setup. If your setup builder is NSIS, use `/S`. If it is MSI, use `/quiet /norestart`.

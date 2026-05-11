# Nilo Desktop Releases

Public release feed for the Nilo AI Desktop installer binaries.

Consumed by `niloai.org/api/desktop/releases` to serve the Windows / macOS / Linux installers from `niloai.org/desktop/download`.

---

## How releases are published

1. **Build** the installer locally in the main repo (`mg7arg/nilo.ai`) under `apps/desktop/`.
2. **Create a release** here at `mg7arg/nilo-desktop-releases` with a tag like `desktop-v0.1.0` (must start with `desktop-v`).
3. **Upload binaries** as release assets. The release page detects platforms by file extension:
   - `.exe` / `.msi` → Windows
   - `.dmg` / `.pkg` → macOS
   - `.AppImage` / `.deb` / `.rpm` → Linux
4. **(Optional)** include SHA-256 hashes in the release body. Convention: a line per asset like `Nilo-AI-Desktop-Setup-0.1.0.exe: a1b2c3...64hex`. The dashboard surfaces the hash for users to verify integrity.

---

## Why a separate public repo

The main `mg7arg/nilo.ai` repo is **private**. Anonymous browsers cannot list private repo releases without a `GITHUB_TOKEN`, so the niloai.org download endpoint either needs (1) a public release repo (this one — no token needed, simpler infra), or (2) the private repo plus a server-side token. We chose option 1: source stays private, release binaries are public (they are distributed to end users anyway).

---

## Discoverability

- List latest release: `GET https://niloai.org/api/desktop/releases` (auth-gated to active org).
- Download proxy: `GET https://niloai.org/api/desktop/download?tag=desktop-v0.1.0&asset=<name>` (auth-gated, redirects to GitHub signed URL).
- UI: `https://niloai.org/desktop/download`.

---

## Versioning

`desktop-v<MAJOR>.<MINOR>.<PATCH>` (semver). Pre-releases use a suffix (e.g. `desktop-v0.2.0-beta`) and are flagged `prerelease: true` on GitHub — the niloai.org endpoint ignores them by default.

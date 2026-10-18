# Create: Ultimate Engineering

A Create-focused Minecraft modpack project for CurseForge. The existing CurseForge profile is the source of truth for the initial mod list and version.

## GitHub → CurseForge automation

The GitHub Actions workflow at `.github/workflows/curseforge.yml` validates a CurseForge-exported manifest, builds a CurseForge-compatible ZIP, stores the ZIP as an Actions artifact, and automatically uploads it to the CurseForge project after a successful build once publishing credentials are configured.

### One-time setup from your existing CurseForge profile

1. In the CurseForge App, open your **Create: Ultimate Engineering** profile.
2. Open the profile menu and choose **Export Profile** / **Share Profile** (wording varies by app version).
3. Export the profile. Do not manually create the manifest; the export records the real Minecraft version, mod loader, and CurseForge project/file IDs.
4. In this repository, create `pack/manifest.json` using the exported `manifest.json`.
5. Copy the exported `overrides/` directory to `pack/overrides/`. Keep custom configs, scripts, resource packs, and other pack-owned files in this folder. Do not commit mod JARs unless you have a specific reason and permission to redistribute them.
6. Commit and push the `pack/` folder to `main`. The workflow builds a ZIP and uploads it as an artifact. Until the CurseForge credentials below are configured, publishing is intentionally skipped.

CurseForge's export format requires `manifest.json` at the ZIP root and pack-specific files under `overrides/`. The workflow assembles this structure automatically.

### Enable automatic CurseForge publishing

After the CurseForge modpack project exists and you have permission to upload files:

1. In GitHub, open **Settings → Secrets and variables → Actions**.
2. Add a **repository secret** named `CURSEFORGE_API_KEY` containing your CurseForge API token.
3. Add a **repository variable** named `CURSEFORGE_PROJECT_ID` containing the numeric project ID from the CurseForge project page URL.
4. Push a change to `pack/**` on `main`, or run the workflow manually from **Actions → Build and publish CurseForge modpack → Run workflow**.

Never put the API token in a tracked file, workflow YAML, issue, or chat. Rotate it immediately if it is exposed.

### What triggers an upload?

- A push to `main` that changes files under `pack/` or the workflow file.
- A manual workflow run.

Every successful eligible run with both publishing settings configured uploads a new **Beta** file to the configured CurseForge project. This is automatic publishing, so only push pack changes you are ready to publish. Change `releaseType` in the workflow to `release` when you are ready to publish stable versions.

### Important notes

- The repository starts without the actual profile export. The first build will fail until `pack/manifest.json` is added.
- The workflow does not invent mod IDs, file IDs, Minecraft versions, or loader versions. Those must come from the real exported profile.
- The generated ZIP is a CurseForge import/export package; it is not a server pack with all mod JARs bundled.
- Publishing a new CurseForge file does not silently force-update existing player profiles. Players may need to update the pack through the CurseForge App.

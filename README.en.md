# Marinara Docs Tutor Lorebook Extension

*[한국어 안내는 여기](./README.md)*

An extension for Marinara Engine that **reads the bundled official docs and
auto-generates a beginner-friendly knowledge lorebook**, then attaches it to
the characters you pick.

Characters with the lorebook attached can answer questions about Marinara
features in chat ("what is a lorebook?", "how do presets work?") grounded in
the actual documentation.

- The docs themselves are **never modified**
- Re-running **updates the entries in place** (no duplicates)
- Lorebooks this extension did not create are **never touched**

## Supported versions

| Item | Details |
|---|---|
| This extension | v1.3.0 |
| Verified host | **Marinara Engine 2.5.0** |
| Verified docs | The official docs bundled with that Marinara 2.5.0 (`docs/`, English originals) |
| Install range | The manifest sets no minimum version. 2.4.x uses the same extension system but is untested; stable 2.3.4 and earlier do not work (different extension system) |
| Verified on | 2026-10-20 |

- **Installation and runtime were verified on Marinara Engine 2.5.0 only.** 2.4.x and releases newer than 2.5.0 may install but are untested; changes to the extension system or the docs layout can break it. If something goes wrong, please include your Marinara version in the report.
- The docs are read from your installed Marinara, so updating Marinara can change the result.

## Install (four safety gates)

Extensions are code that runs inside the app, so Marinara deliberately makes
you pass several gates. In order:

### 1. Add one line to `.env`

Quit Marinara completely, open a terminal (or PowerShell) in your install
folder, and run:

Windows (open the install folder in Explorer → type `powershell` in the
address bar → Enter):

```powershell
Add-Content .env -Value "", "ENABLE_EXTERNAL_EXTENSIONS=true"
```

macOS / Linux / Termux (from the install folder):

```sh
echo "ENABLE_EXTERNAL_EXTENSIONS=true" >> .env
```

Then start Marinara again.
> **🐳 Using Docker?** — edit `docker-compose.yml` instead of `.env`.
> Find the `environment:` line and add **one line** under it (match the
> indentation of its neighbors):
>
> ```yaml
>       - ENABLE_EXTERNAL_EXTENSIONS=true
> ```
>
> Save, then run `docker compose up -d`.
> If you access Marinara from another device's browser, also add
> `- ADMIN_SECRET=yoursecret` in the same place (needed to open the Danger Zone).
> To confirm it took effect:
>
> ```sh
> docker compose exec marinara env | grep ENABLE_EXTERNAL
> ```
>
> If the value prints, it's applied; if not, run `docker compose up -d` once more.


### 2. Enable the in-app toggle

**Settings → Advanced → Danger Zone → Allow third-party extension imports**.

### 3. Import the extension — pick one

#### Option A: GitHub Extension Manager (recommended, easy updates)

If the "GitHub 확장 관리자" (GitHub Extension Manager) extension is already
installed and running:

1. Press the **GitHub 확장 관리** button under
   **Settings → Addons → External Extensions**
2. Enter `https://github.com/mallang0723/ME-docsTutor` and press
   **패키지 찾기** (find packages)
3. Choose **`extension/manifest.json`** as the target
   > A **release entry** such as `v1.2.0 / marinara-docs-tutor.zip` may also be
   > listed, possibly preselected. Browsers block downloading release files
   > (CORS), so make sure you pick `extension/manifest.json`.
4. **내용 확인** (review) → **설치** (install). It is saved disabled; continue
   with step 4.

#### Option B: Import a ZIP file

Use the zip from this repository's **Code → Download ZIP** (no need to unzip
it). If the [Releases page](../../releases/latest) has a zip, that works too, but
releases can lag behind the repository.

**Settings → Addons → External Extensions → Import Extension File** → pick the
**zip file as-is**.

### 4. Approve and enable — nothing runs until you do this

Press the power icon (Review and run) on the imported extension. An approval
dialog appears.

> This extension draws a button on the page, so it requests **full page
> access**. That permission means the extension can read and change
> everything you see in Marinara — which is why the app warns you loudly.
> Only click **"Run Exact Code"** if you trust the code.

Whenever the extension file changes (e.g. an update), the same dialog
reappears. That is by design: changed code must be re-approved.

## Updating

- **Installed with the manager:** GitHub 확장 관리 → **업데이트 확인** (check)
  on this extension → **업데이트 보기** → **업데이트 설치** → approve again as
  in step 4 → reload the page.
- **Installed earlier from a ZIP:** keep it. In the manager, use
  **GitHub 주소 연결** (link GitHub address) on this extension → enter the URL
  above → choose **`extension/manifest.json`** → **이 저장소 연결**. Then update
  as above. The extension name (`Marinara Docs Tutor Lorebook`) is unchanged,
  so it links to the existing install and keeps its stored data (the record of
  the lorebook it created, the button position).
- **Staying on ZIPs:** import the new zip the same way; confirm
  **Save Disabled Update**, then approve again.

## Usage

1. Click the **📚 button** that appears on screen (drag to move it;
   the position is remembered)
2. **Select the characters** that should get the lorebook
3. Click **"Generate + attach"** and wait for the completion report

Run it again after updating Marinara and the lorebook refreshes against the
current docs.

## What changed for 2.5.0 (v1.3.0)

- Verified on Marinara v2.5.0: install, approval, lorebook generation,
  re-run (entry refresh), character attach and keyword activation. The
  version warning no longer appears.
- Since v2.5.0 Marinara can switch the docs language (downloaded translation
  packs). With e.g. Korean docs active, the extension used to read the
  translations and produce a different lorebook. It now **always reads the
  English originals**, regardless of the docs language setting (Korean
  synonym keywords are still added).
- The manifest now uses the current `marinara.personal-extension` format and
  states its version (`1.3.0`) and its permission (full page access)
  explicitly. The name is unchanged, so existing installs can be updated.
- The code changed, so you will need to approve it once more after updating.

## Troubleshooting

| Symptom | What to do |
|---|---|
| No import menu | Check gates 1–2, then restart Marinara |
| Manager says "GitHub 연결이 차단되었거나…" (connection blocked) | You picked the release entry. Choose `extension/manifest.json` |
| Manager says "같은 이름의 확장이 있습니다" (same name exists) | It is already installed from a ZIP. Use **GitHub 주소 연결** on that entry instead of installing again |
| Approved but no 📚 button | Refresh the page. If it persists, share the red errors from the browser console (F12) |
| "Could not read docs" | Your Marinara is outdated or the docs layout changed — check for an extension update |
| "…already exists but cannot be verified…" | You have a lorebook with the same name that this extension didn't create. Rename or delete it, then re-run |
| Stopped mid-run after disabling the extension | Already-completed server changes are not rolled back. Re-enable and re-run to reconcile |

## Good to know

- The extension only talks to your own Marinara server (same machine).
  It sends nothing anywhere else.
- The full source lives in this repository. The hash in the approval dialog
  lets you confirm you are running exactly this code.

## License

MIT

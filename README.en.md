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

## Requirements

- **Marinara Engine 2.3.5 (staging)** — does not work on stable 2.3.4,
  which ships a different extension system.

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


### 2. Enable the in-app toggle

**Settings → Advanced → Danger Zone → Allow third-party extension imports**.

### 3. Import the extension

First download `marinara-docs-tutor.zip` from the
[**Releases page**](../../releases/latest) — no need to unzip it.

**Settings → Addons → External Extensions** → import the **zip file
as-is**.

### 4. Approve and enable — nothing runs until you do this

Press the power icon on the imported extension. An approval dialog appears.

> This extension draws a button on the page, so it requests **full page
> access**. That permission means the extension can read and change
> everything you see in Marinara — which is why the app warns you loudly.
> Only click **"Run this exact code"** if you trust the code.

Whenever the extension file changes (e.g. an update), the same dialog
reappears. That is by design: changed code must be re-approved.

## Usage

1. Click the **📚 button** that appears on screen (drag to move it;
   the position is remembered)
2. **Select the characters** that should get the lorebook
3. Click **"Generate + attach"** and wait for the completion report

Run it again after updating Marinara and the lorebook refreshes against the
current docs.

## Troubleshooting

| Symptom | What to do |
|---|---|
| No import menu | Check gates 1–2, then restart Marinara |
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

# NotchBar Releases

Compiled Windows installers for [NotchBar](https://notchbar-landing.vercel.app) — a Dynamic
Island-style desktop overlay. This repo holds release binaries only; the application source
is closed. Each release's notes are the changelog.

## Download

Buy a license and download the installer at
[notchbar-landing.vercel.app/download](https://notchbar-landing.vercel.app/download).
NotchBar needs a license key to run; one key covers 3 devices, and you can free a slot again
from **Settings → About → Deactivate this device**.

The installers are also attached to every entry on the
[Releases page](https://github.com/MrCyberNaut/notchbar-releases/releases/latest):
`.exe` is the normal installer, `.msi` is for winget and managed deployments.

## Windows security prompts

NotchBar is not code-signed yet, so Windows will complain the first time:

- **SmartScreen** ("Windows protected your PC") — click **More info → Run anyway**.
- **Smart App Control**, if it is turned on, blocks unsigned apps outright and offers no
  "Run anyway". Microsoft provides no per-app exception, so the only options are turning it
  off (Windows Security → App & browser control → Smart App Control) or waiting for a signed
  build. Check before you buy — the [download page](https://notchbar-landing.vercel.app/download)
  explains how.

## Verifying a download

Each release lists the SHA-256 checksum of its installer. Compare it before running:

```powershell
Get-FileHash .\NotchBar_<version>_x64-setup.exe
```

## Updates

NotchBar checks this repo's `latest.json` for new versions. Checking is manual: open
**Settings → About → Check for updates**. Nothing is downloaded or installed in the
background.

## Support and policies

- Support: [arnavsawant.ai@gmail.com](mailto:arnavsawant.ai@gmail.com)
- [Refund policy](https://notchbar-landing.vercel.app/refunds) — 7 days, no questions asked
- [Terms](https://notchbar-landing.vercel.app/terms) ·
  [Privacy](https://notchbar-landing.vercel.app/privacy)

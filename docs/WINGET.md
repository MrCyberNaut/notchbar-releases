# Publishing NotchBar to winget

`.github/workflows/winget.yml` keeps the winget manifest in sync with releases in
this repo. It runs `vedantmgoyal9/winget-releaser@v2` whenever a GitHub release is
**published**, which reads the release's `.msi` asset, hashes it, and opens a
pull request against [`microsoft/winget-pkgs`](https://github.com/microsoft/winget-pkgs).

## The action only *updates* an existing package

This is the part that trips people up: `winget-releaser` can bump a package that is
already in the winget community repository, but it cannot create one. The very first
version has to be submitted by hand.

Do it once, from a machine with the MSI URL to hand:

```powershell
winget install wingetcreate
wingetcreate new https://github.com/MrCyberNaut/notchbar-releases/releases/download/v0.1.1/NotchBar_0.1.1_x64_en-US.msi
```

(`komac new <msi-url>` does the same job if you prefer it.) `wingetcreate` interviews
you for the metadata, generates the three manifest files, and opens the PR.

### Manifest values to use

| Field | Value |
|---|---|
| `PackageIdentifier` | `ArnavSawant.NotchBar` |
| `Publisher` | `Arnav Sawant` |
| `PackageName` | `NotchBar` |
| `License` | `Proprietary` |
| `InstallerType` | `wix` |
| `Scope` | *omit* — see below |
| `ShortDescription` | `Dynamic Island for Windows` |
| `PackageUrl` | `https://notchbar-landing.vercel.app` |

`PackageIdentifier` must match the `identifier:` in the workflow exactly, or every
later automated update will target a package that does not exist.

**On `Scope`:** `src-tauri/tauri.conf.json` sets `nsis.installMode: "currentUser"`, but
that only governs the NSIS `.exe`. The WiX/MSI bundle has no scope override there, so it
builds per-machine by default. Verify against the actual MSI before claiming otherwise —
and if it is per-machine, leave `Scope` out of the manifest rather than asserting `user`.

## Repository secret

The workflow needs `WINGET_TOKEN` in this repo's Actions secrets:

1. A **classic** GitHub personal access token with the `public_repo` scope.
   (A fine-grained token will not work here — the action pushes a branch to your fork.)
2. A fork of `microsoft/winget-pkgs` under the `MrCyberNaut` account. The action pushes
   the manifest branch there and opens the PR from it.

Without both, the workflow fails at the push step.

## Do not submit before v0.1.1

v0.1.0 validates license keys against Dodo's **test** host, so every real key a customer
buys comes back invalid in that build. Submitting it to winget would put a broken binary
in front of `winget install` users. Wait for v0.1.1 — the first build that selects the
live Dodo host — and make the manual `wingetcreate new` submission against that version.

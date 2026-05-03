# Whisper Dictate — Public Distribution Channel

Public companion to the (internal) [`Neko-Health/whisper-dictate`](https://github.com/Neko-Health/whisper-dictate) repository. Hosts the **update manifest** (`latest.json`) and **release assets** (`WhisperDictateSetup-vX.Y.Z.exe`) so installed copies of the app can check for updates without authenticating to GitHub.

This repository contains **no source code**. Source lives on the internal repo.

## What's here

- **`latest.json`** — manifest read by installed copies of Whisper Dictate. Polled approximately once a day by the running app to detect new versions. Schema documented in `latest.json` itself.
- **GitHub Releases** — each release attaches the corresponding `WhisperDictateSetup-vX.Y.Z.exe` installer. The manifest's `installer_url` field points at the asset URL of the most recent release.

## How updates flow

```
Maintainer (Oscar) ──build── WhisperDictateSetup-vX.Y.Z.exe
                  │
                  ├──tag───► Neko-Health/whisper-dictate (internal, source)
                  │
                  └──release──► oscarnorberg/whisper-dictate-public
                                ├── Release vX.Y.Z (asset: .exe)
                                └── latest.json updated to point at vX.Y.Z

Installed app on user's machine
  └── once a day, GET raw.githubusercontent.com/.../latest.json (anonymous)
      └── if newer version → notify user, offer one-click update
          └── download the .exe asset → run installer → app upgraded
```

## Trust & maintenance

- **Single maintainer:** [@oscarnorberg](https://github.com/oscarnorberg). 2FA-enforced personal account. If this account is ever compromised, an attacker could push a malicious manifest pointing at a malicious installer; existing installs would auto-upgrade to it. Mitigation: 2FA + hardware key. Future hardening: code signing + manifest signing (deferred to v0.4+).
- **Bus factor:** if Oscar leaves Neko Health or otherwise can't maintain this, existing installs will continue working but will silently stop seeing updates. Procedure to migrate: transfer this repo to a new maintainer and ship a new version of Whisper Dictate that points the manifest URL at the new location. Existing installs on the old URL will need a manual reinstall to follow the migration.
- **Why a personal account:** Neko Health does not currently host public artifact channels. This personal-account repo is an interim arrangement; the long-term home would be a public repo under `Neko-Health` org or a Neko-managed Azure Blob if/when that becomes available.

## License

The Whisper Dictate installer artifacts hosted here are subject to the license of the source project. See the source repository for details.

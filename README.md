# compton-releases

Release binaries for command-line tools built in a private monorepo. This repo holds releases only: there's no source code here.

## orch-tui

A read-only terminal board for an orchestrator's task state. Linux x86_64 (a static musl binary that also runs on WSL2).

Install, or reinstall, the newest release into `~/.local/bin`:

```bash
curl -fsSL https://raw.githubusercontent.com/rcompton78/compton-releases/main/install.sh | bash -s orch-tui
```

A failed download pipes nothing to bash, which then exits 0, so check that it printed `Installed orch-tui <version>.` Set `INSTALL_DIR` to install somewhere else.

Once installed, it keeps itself up to date: it checks for a newer release at launch and every 5 minutes, and installs it when you quit. `orch-tui update` installs one straight away. `ORCH_TUI_AUTO_UPDATE=0` only shows the notice, and `ORCH_TUI_UPDATE_INTERVAL` sets the minutes between checks (`0` checks only at launch).

No GitHub account or token is needed. Without one, GitHub allows 60 API requests an hour per IP address; set `ORCH_TUI_GH_TOKEN` (or `GH_TOKEN`) to raise that.

## Verifying a release

Each release has the tarball, its `.sha256` and an `install.sh` with the app baked in. Tarballs are signed (ed25519, in [zipsign](https://github.com/Kijewski/zipsign)'s format, appended to the `.tar.gz`), and every update checks the signature against the public key built into the installed binary before replacing it:

```
YwQr9aRnQ5yqqWsZj+HVPQ0EYSXMWFX3JnPbdOy1ZW8=
```

The first install trusts this repo and checks the `.sha256` and the new binary's `--version`; every update after that also checks the signature. Release notes name the source commit each build came from.

This file and the root `install.sh` are kept in step by the release pipeline, so edits here are overwritten.

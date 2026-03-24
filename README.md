# codexm

`codexm` (Codex Manager) is a small CLI tool for running multiple Codex accounts/profiles with separate homes.

It uses one wrapper command per profile (for example `codex-work`), sets `CODEX_HOME` to an isolated directory, and forwards to the original `codex` binary.

## Quick Start

```bash
chmod +x ./codexm
./codexm install
codexm doctor
```

After `install`, `codexm` is copied to `~/.local/bin/codexm` by default, so you can run it from anywhere.

Then create a profile wrapper:

```bash
codexm add work
codex-work
```

During `add`, you will get 2 interactive prompts:
1. Copy plugins/skills from source home?
2. Copy current config (`auth.json` + `config.toml`)?

## Commands

```bash
# Install codexm to local bin (default: ~/.local/bin/codexm)
./codexm install

# Optional: custom binary name and bin dir
./codexm install codexm /custom/bin

# Create a wrapper command (default: codex-<profile>)
codexm add <profile>

# Create with custom wrapper name
codexm add <profile> <command-name>

# Run codex once with a profile (without creating wrapper)
codexm run <profile> [codex args...]

# List profiles from profile root
codexm list

# Print absolute path of a profile home
codexm path <profile>

# Remove wrapper command only (keeps profile data)
codexm remove <profile> [command-name]

# Show environment and sanity checks
codexm doctor
```

## Environment Variables

- `CODEXSWITCH_PROFILE_ROOT`: profile root directory (default: `~/.codex/profiles`)
- `CODEXSWITCH_BIN_DIR`: output directory for wrappers and install target default (default: `~/.local/bin`)
- `CODEXSWITCH_CODEX_CMD`: original Codex command (default: `codex`)
- `CODEXSWITCH_SOURCE_HOME`: source directory used by `add` copy prompts (default: `$CODEX_HOME` or `~/.codex`)
- `CODEXSWITCH_MANAGER_BIN_NAME`: default install binary name (default: `codexm`)

## Example

```bash
codexm add personal
codexm add work

codex-personal
codex-work
```

Each wrapper uses a different `CODEX_HOME`, so auth/config/history stay isolated.

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

During `add`, you will get interactive prompts for:
1. Copy plugins/skills from source home?
2. Copy current config (`auth.json` + `config.toml`)?
3. Optionally migrate sessions into the new profile

The session migration flow supports:
- Copy all sessions from the current source home/profile into the target profile
- Copy one session by selecting it from recent sessions or entering a session id manually
- Picking the source from `source home` or any profile under `~/.codex/profiles`

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

# Import one session from default ~/.codex into current home/profile
codexm import <session-id> [target-profile|@current]

# Interactive session migration into current home/profile
codexm migrate session

# Copy all sessions from one profile into current home/profile
codexm migrate session copy <source-profile|@source> [target-profile|@current]

# Copy one session from one profile into current home/profile
codexm migrate session one <source-profile|@source> <session-id> [target-profile|@current]

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

`migrate session` defaults the target to the current `CODEX_HOME` when available, otherwise to `CODEXSWITCH_SOURCE_HOME`. Use `@source` to refer to the configured source home and `@current` to refer to the current home.

`codexm import <session-id>` is a shortcut for the common case of copying one session from default `~/.codex` into the current `CODEX_HOME`.

# PathSmith: Dynamic PATH Manager

## Purpose

PathSmith dynamically manages your shell's `PATH` environment variable and other shell-specific configurations by loading, validating, and merging directory paths from a single, clean configuration file.

## Supported Shells

- **`init` command**: posix shell family (sh, ash, bash, zsh), nushell, fish, PowerShell (pwsh)
- **`config` command**: fish (currently)

## Usage

PathSmith now uses subcommands: `init` and `config`.

```sh
pathsmith [-v|--verbose] [-p|--paths PATHFILE] <init|config> <shell> [options]
```

### `init` Subcommand

The `init` subcommand generates shell code to set your `PATH` for the current session. It merges paths from your `pathsmith.conf` file with your existing `PATH`.

You should evaluate this command in your shell's configuration file (e.g., `.bashrc`, `.zshrc`, `config.fish`).

- **Example for bash:**
  ```sh
  eval -- $(pathsmith init bash)
  ```

- **Example for zsh:**
  ```sh
  eval -- $(pathsmith init zsh)
  ```

- **Example for fish:**
  ```fish
  pathsmith init fish | source
  ```

- **Example for nushell:**
  ```sh
  pathsmith init nu | save ~/.config/nushell/pathsmith.nu; source ~/.config/nushell/pathsmith.nu
  ```

- **Example for PowerShell (pwsh):**
  ```powershell
  Invoke-Expression -Command $(pathsmith init pwsh)
  ```

- **Example for list (show the final list of paths, one per line):**
  ```sh
  pathsmith init list
  ```

### `config` Subcommand

The `config` subcommand provides persistent, shell-specific configuration.

#### Fish Shell

For `fish`, this command directly modifies the `fish_variables` file to persistently set `fish_user_paths`. This is the recommended way to manage your path in Fish.

```sh
# Run a dry-run to see what changes would be made
pathsmith config fish

# Apply the changes after reviewing
pathsmith config fish --apply
```

The command will also prompt for confirmation if `--apply` is not provided.

## Arguments

### Global Options
- `-v`, `--verbose`: Enable verbose output (writes details to stderr).
- `-p PATHFILE`, `--paths PATHFILE`: Specify a custom paths file. Defaults to `~/.config/pathsmith.conf` if not provided.
- `-h`, `--help`: Show usage information.

### Subcommands
- `init <shell>`: Generates shell code to set `PATH` for the current session.
- `config <shell>`: Manages persistent configuration for the specified shell.
  - `--apply`: (For `config` command) Apply changes without prompting for confirmation.

## Configuration

- The default paths file is `~/.config/pathsmith.conf`.
- The paths file can contain:
  - Absolute paths (one per line)
  - Comments (lines starting with `#`)
  - Blank lines (ignored)
  - Environment variables (e.g., `$HOME`)
  - Tilde (`~`) for home directory

## Troubleshooting

- Use `-v` or `--verbose` to see which paths are added or skipped (output goes to stderr).
- Common issues:
  - Paths file missing: Check the file path or specify a different one.
  - Invalid or non-existent directories: These are skipped and optionally logged in verbose mode.

## Requirements

- Ruby (standard library only)
- Compatible with bash, zsh, nushell, fish, and PowerShell (pwsh)

## Example paths file

pathsmith.conf:

```shell
# This file contains a plain list of directories (one per line) to be included in the PATH environment variable.
# It is not a shell script. Use a loader script in your shell configuration to process and export these paths.

~/bin
~/.cargo/bin
~/.go/bin
~/.local/bin
$HOME/.nix-profile/bin
/opt/homebrew/bin
/opt/homebrew/sbin
```

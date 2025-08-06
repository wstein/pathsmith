# PathSmith: Dynamic PATH Merger

## Purpose

PathSmith dynamically manages your PATH environment variable by loading, validating, and merging directory paths from a configurable file.

## Supported Shells

- posix shell family (sh, ash, bash, zsh)
- nushell
- fish
- PowerShell (pwsh)

## Usage

1. Run the script to generate a shell script for your shell (bash, zsh, nushell, fish, pwsh):
   ```sh
   pathsmith [-v|--verbose] [-p|--paths PATHLIST] SHELL
   ```
   - Example for bash:

     ```sh
     eval -- $(pathsmith bash)
     ```

   - Example for zsh:

     ```sh
     eval -- $(pathsmith zsh)
     ```

   - Example for list (show the final list of paths, one per line):

     ```sh
     pathsmith list
     ```

   - Example for nushell:

     ```sh
     pathsmith nu | save ~/.config/nushell/pathsmith.nu; source ~/.config/nushell/pathsmith.nu
     ```

     > **Note:** Nushell does not support `eval`/`source` in the same way as POSIX shells. A different approach may be needed for full integration in the future.

   - Example for fish:

     ```fish
     pathsmith fish | source
     ```

   - Example for PowerShell (pwsh):

     ```powershell
     Invoke-Expression -Command $(pathsmith pwsh)
     ```

## Arguments

- `-v`, `--verbose`: Enable verbose output (writes details to stderr).
- `-p PATHFILE`, `--paths PATHFILE`: Specify a custom paths file. Defaults to `~/.config/pathsmith.conf` if not provided.
- `SHELL`: Target shell for output (e.g., `bash`, `zsh`, `nu`, `fish`, `pwsh`, `list`).
- `-h`, `--help`: Show usage information.

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

# Changelog

All notable changes to xSHELL are documented here.

The format follows the spirit of Keep a Changelog, and versions use semantic versioning.

## [0.5.0] - 2026-06-12

### Added

- Added `ln` for symlink management.
- Added `ln ls` for listing symlinks.
- Added `ln broken` for finding broken symlinks.
- Added preview-first `ln rm` for batch unlinking symlinks.

## [0.4.1] - 2026-06-11

### Added

- Added `has -t f|d|a` for checking files, directories, or any path type.

## [0.4.0] - 2026-06-10

### Added

- Added `has` for checking which child directories contain or miss a named file.
- Supports direct checks by default, recursive checks with `-r`, and depth-limited checks with `-D N`.

## [0.3.0] - 2026-05-23

### Added

- Added `find -c TEXT` / `find --contains TEXT` to keep files whose contents contain a fixed string.
- Added `find --count` to print fixed-string match counts with `-c`.

## [0.2.0] - 2026-05-23

### Added

- Added `ren` for preview-first batch file renaming.
- Supports stripping filename suffixes with `-x`.
- Supports changing extensions with `-e OLD:NEW`.
- Supports text replacement with `-f OLD -t NEW`.

## [0.1.0] - 2026-05-23

### Added

- Initial `xSHELL` Bash CLI.
- Added `ext` for checking child directories by file extension.
- Added `find` for recursive file or directory search with sorting.
- Added `path` for storing and printing frequently used paths.
- Added `size` for child directory disk usage summaries.
- Added `top` for process snapshots by user, PID, CPU, memory, command text, and watch mode.
- Added English and Chinese README files.
- Added `version`, `-v`, and `--version` output.

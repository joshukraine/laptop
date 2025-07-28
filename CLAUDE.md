# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a shell script project that automates macOS development environment setup. It's a personal fork of thoughtbot's laptop setup script with specific customizations for the maintainer's workflow.

**Target Platform**: macOS (Sequoia 15.x, Sonoma 14.x, Ventura 13.x, Monterey 12.x)
**Architecture Support**: Both Apple Silicon (arm64) and Intel (x86_64)

## Development Commands

### Primary Usage

```bash
# Execute the main script with logging
sh mac 2>&1 | tee ~/laptop.log
```

### Code Quality

```bash
# Lint the shell script (primary quality check)
shellcheck mac

# Syntax validation
bash -n mac
```

### Testing Strategy

- Test on fresh macOS installations using virtualization (UTM recommended)
- Create VM snapshots for repeatable testing
- Script is idempotent - safe to run multiple times

## Code Architecture

### Main Script Structure (`mac`)

The 216-line main script follows this pattern:

1. **Setup Phase**: Error handling (`set -e`), architecture detection, Homebrew installation
2. **Core Installation**: Essential development tools via Homebrew
3. **Language Setup**: asdf version manager with Ruby and Node.js
4. **Customization**: Sources user's `~/.laptop.local` if present

### Key Functions

- `fancy_echo()` - User feedback with consistent formatting
- `append_to_zshrc()` - Shell configuration management
- `gem_install_or_update()` - Ruby gem handling
- `add_or_update_asdf_plugin()` - Language version manager setup
- `install_asdf_language()` - Language installation wrapper

### Fork-Specific Customizations

- **asdf via Homebrew**: Uses Homebrew installation method for better maintainability
- **Minimal package selection**: thoughtbot/heroku tools and taps commented out
- **Modern editor**: Uses Neovim instead of Vim
- **Tmux included**: Enabled for terminal multiplexing workflow
- **Streamlined toolset**: RCM, Watchman, reattach-to-user-namespace, GPG Suite commented out
- **PostgreSQL version pinned to 14**: Specific version for compatibility

## Repository Structure

This is a **fork** with upstream relationship:

- **Origin**: `https://github.com/joshukraine/laptop.git`
- **Upstream**: `https://github.com/thoughtbot/laptop.git`

### Important Files

- `mac` - Main executable script
- `README.md` - Comprehensive usage documentation
- `CHANGELOG` - Required for all changes per PR template
- `.github/PULL_REQUEST_TEMPLATE.md` - Enforces CHANGELOG and README updates
- `CLAUDE.md` - Development guidance for Claude Code
- `SECURITY.md` - Security policy documentation

### CI/CD Setup

- **No CI workflows**: Thoughtbot-specific automated workflows removed
- **Manual quality checks**: Use `shellcheck mac` for validation
- **UTM testing**: Test changes on fresh macOS installations

## Shell Scripting Conventions

- Uses `set -e` with trap for error handling
- Implements architecture detection via `uname -m`
- All variables properly quoted for shellcheck compliance
- Idempotent operations throughout (safe to re-run)
- Comprehensive logging to `~/laptop.log`

## Architecture Handling

The script detects Intel vs Apple Silicon and adjusts:

- Homebrew installation paths (`/opt/homebrew` vs `/usr/local`)
- Rosetta 2 installation for Apple Silicon
- Architecture-specific Homebrew prefix in shell configuration

## Quality Standards

- All shell code must pass `shellcheck mac`
- Follow thoughtbot's open source code of conduct
- PR template requires CHANGELOG and README updates
- Test on clean macOS installations before merging

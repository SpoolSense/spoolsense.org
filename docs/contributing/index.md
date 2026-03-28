# How to Contribute

SpoolSense is open source (GPL-3.0) and welcomes contributions. Here's how to help.

## Ways to Contribute

### Report Bugs

Found something broken? [Open an issue](https://github.com/SpoolSense/spoolsense_scanner/issues) with:

- What you expected to happen
- What actually happened
- Steps to reproduce
- Your hardware (board, NFC reader, tag type)
- Firmware version (shown on the landing page)

### Test and Give Feedback

Build a scanner, try it out, tell us what works and what doesn't. Even "it just worked" is helpful feedback.

### Write Documentation

This site is built from Markdown files in the [spoolsense.org repo](https://github.com/SpoolSense/spoolsense.org). Fork it, edit, and open a PR.

### Design an Enclosure

We need printable cases. See [Enclosure Design](enclosure-design.md) for what's needed and how to submit.

### Contribute Code

See [Development Setup](dev-setup.md) to get started with the codebase.

## Repositories

| Repo | Language | What to Work On |
|------|----------|----------------|
| [spoolsense_scanner](https://github.com/SpoolSense/spoolsense_scanner) | C++ (Arduino) | Firmware, NFC, web UI |
| [spoolsense_middleware](https://github.com/SpoolSense/spoolsense_middleware) | Python | Klipper/AFC integration |
| [spoolsense-installer](https://github.com/SpoolSense/spoolsense-installer) | Python/Bash | Setup experience |
| [spoolsense.org](https://github.com/SpoolSense/spoolsense.org) | Markdown | Documentation |

## Workflow

1. Fork the repo
2. Create a branch from `dev` (not `main`)
3. Make your changes
4. Open a PR targeting `dev`
5. CodeRabbit will auto-review your PR

`main` is the production branch. All work goes through `dev` first.

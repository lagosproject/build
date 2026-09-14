<h2 align="center">
  <a href=#><img src="https://raw.githubusercontent.com/armbian/.github/master/profile/logosmall.png" alt="Armbian logo"></a>
  <br><br>
</h2>

# Armbian Linux Build Framework

## Purpose of This Repository

The **Armbian Linux Build Framework** creates customizable OS images based on **Debian** or **Ubuntu** for **single-board computers (SBCs)** and embedded devices. It builds a complete Linux system — kernel, bootloader, and root filesystem — with full control over versions, configuration, firmware, device trees, and system tuning.

The framework supports **native**, **cross**, and **containerized** builds across multiple architectures (`x86_64`, `aarch64`, `armhf`, `riscv64`), and is suitable for development, testing, production, and automation pipelines.

> **Looking for prebuilt images?** Use [Armbian Imager](https://github.com/armbian/imager/releases) — the easiest way to download and flash Armbian to an SD card or USB drive. Available for Linux, macOS, and Windows.

## Quick Start

```bash
git clone https://github.com/armbian/build
cd build
./compile.sh
```

<a href="#quick-start"><img src=".github/README.gif" alt="Build demonstration" width="100%"></a>

## Build Host Requirements

### Hardware
- **RAM:** ≥8 GB (less with `KERNEL_BTF=no`)
- **Disk:** ~50 GB free space
- **Architecture:** `x86_64`, `aarch64`, or `riscv64`

### Operating System
- **Native builds:** Armbian/Debian 13 (Trixie)
- **Containerized:** any Docker-capable Linux
- **Windows:** WSL2 with Armbian/Debian 13 (Trixie)

### Software
- Superuser privileges (`sudo` or root)
- An up-to-date host (outdated Docker or other tooling can cause build failures)

## Built With

- **Bash** — the build framework itself (`compile.sh` and the scripts under `lib/`, `extensions/`, and `tools/`)
- **Python** — helpers and generators invoked by the build
- **Board / family / kernel configuration** — plain shell-sourced files under `config/`
- **Patches, kernel configs, overlays, and package sources** — under `patch/`, `config/kernel/`, and `packages/`
- **YAML** — GitHub Actions workflows and repository metadata
- **GitHub composite action** — see [`action.yml`](action.yml), which wraps the framework for use from other workflows

## Repository Layout

| Path            | Description                                                                                          |
|-----------------|------------------------------------------------------------------------------------------------------|
| `compile.sh`    | Main entrypoint that bootstraps the build via `lib/single.sh` and dispatches to the CLI.             |
| `action.yml`    | GitHub composite action ("Rebuild Armbian") wrapping the framework for CI reuse.                     |
| `lib/`          | Core build framework: functions, CLI entrypoint, tooling wrappers.                                   |
| `config/`       | Board, family, distribution, kernel, boot-env and CLI configuration (see subdirectory READMEs).      |
| `config/boards/`| Per-board configuration files. File extension encodes support status (see below).                    |
| `patch/`        | Kernel and u-boot patch sets, kernel device-tree overlays and Makefiles, per branch/version.         |
| `packages/`     | Armbian-specific packaging: kernel deb scripts, `bsp`, `bsp-cli`, `bsp-desktop`, blobs, extras.      |
| `extensions/`   | Optional build-time extensions enabled via `ENABLE_EXTENSIONS=`.                                     |
| `tools/`        | Maintainer helper scripts (e.g. `mk_format_patch`, `unifying_configs`).                              |
| `.github/`      | Issue / PR templates, labels, CODEOWNERS generator, and CI workflows.                                |

### Board support levels

Board configuration files under `config/boards/` use their extension to declare support status:

| Extension | Meaning                                        |
|-----------|------------------------------------------------|
| `.conf`   | Supported — current package base               |
| `.csc`    | Community maintained / unstable                |
| `.wip`    | Work in progress                               |
| `.eos`    | End of life                                    |
| `.tvb`    | TV box                                         |

See [`config/boards/README.md`](config/boards/README.md) for the full list of per-board configuration variables (`BOARD_NAME`, `BOARDFAMILY`, `BOOTCONFIG`, `KERNEL_TARGET`, overlays, kernel modules, and more).

## Using as a GitHub Action

This repository ships a composite action (see [`action.yml`](action.yml)) that other workflows can call to build an Armbian image or kernel. Inputs include `armbian_target`, `armbian_board`, `armbian_branch`, `armbian_kernel_branch`, `armbian_release`, `armbian_ui`, `armbian_compress`, `armbian_extensions`, and GPG-signing / release-publishing options.

## Continuous Integration

Repository automation (labeling, board-asset checks, kernel security checks, artifact builds, mirroring, dispatch to forks, etc.) is defined under [`.github/workflows/`](.github/workflows/). A live overview of runs for this repo is available at:

👉 <https://actions.armbian.com/?repo=build>

## Resources

- **[Documentation](https://docs.armbian.com/Developer-Guide_Overview/)** — Comprehensive guides for building, configuring, and customizing
- **[Website](https://www.armbian.com)** — News, features, and board information
- **[Blog](https://blog.armbian.com)** — Development updates and technical articles
- **[Forums](https://forum.armbian.com)** — Community support and discussions

## Contributing

We welcome contributions! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on reporting issues, working on the backlog, labeling PRs, generating patches with `./compile.sh CREATE_PATCHES="yes"`, and submitting pull requests.

Credits are listed in [CREDITS.md](CREDITS.md) and at <https://www.armbian.com/authors>.

## Support

### Community Forums
Get help from users and contributors on troubleshooting, configuration, and development.
👉 [forum.armbian.com](https://forum.armbian.com)

### Real-time Chat
Join discussions with developers and community members on IRC or Discord.
👉 [Community Chat](https://docs.armbian.com/Community_IRC/)

### Paid Consultation
For commercial projects, guaranteed response times, or advanced needs, paid support is available from Armbian maintainers.
👉 [Contact us](https://www.armbian.com/contact)

## Contributors

Thank you to everyone who has contributed to Armbian!

<a href="https://github.com/armbian/build/graphs/contributors">
  <img alt="Contributors" src="https://contrib.rocks/image?repo=armbian/build" />
</a>

## Armbian Partners

Our [partnership program](https://forum.armbian.com/subscriptions) supports Armbian's development and community. Learn more about [our Partners](https://armbian.com/partners).

## License

Released under the [GNU General Public License v2](LICENSE).

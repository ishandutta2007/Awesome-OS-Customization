# Awesome-OS-Customization

# Awesome-OS-Customization

# Top OS Customization Tools Ecosystem

**Curated List of SaaS Products & Open-Source GitHub Projects**  
*Focused on Image Slipstreaming, Bloatware Removal & Bootable Media Creation*  
**Last updated: March 2026**

This repository tracks notable **SaaS platforms** and **open-source projects** for **OS Customization**. These tools let you modify Windows installation images before a single machine boots — stripping bloatware, integrating drivers and updates, pre-configuring settings, and building lightweight ISOs — or tweak a running system after the fact. The spectrum runs from paid image-editing suites to free PowerShell scripts that build trimmed-down ISOs.

**Examples** include NTLite, Tiny11 Builder, Dism++, Winaero Tweaker, Optimizer, Rufus, Ventoy, and WinOptimizer (the category leaders).

**Open-source emphasis**: This section is heavily expanded with every major active project for self-hosting, scriptable image building, and transparent system tweaks — ideal for power users and IT admins who want a lean Windows install without paying for a commercial image editor or trusting a pre-built "Ghost Spectre" ISO from an anonymous uploader.

Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.

## Table of Contents
- [SaaS/Hosted Platforms](#saas-hosted-platforms)
- [Open-Source GitHub Projects](#open-source-github-projects)
- [How to Contribute](#how-to-contribute)
- [Disclaimer](#disclaimer)

## SaaS/Hosted Platforms

- **[NTLite](https://www.ntlite.com/)**  
  **The reference commercial Windows image customizer** from the author of nLite (the Windows XP slipstreaming tool) . **Opens Windows installation files (ISO, WIM, ESD, SWM) and lets you strip components before a single machine boots** . **Slipstream cumulative updates and drivers** so the PC is current on first boot and can see NVMe drives during installation . **Make installs fully unattended** — partitioning, user accounts, and region pre-configured . **Can also edit an already-deployed Windows install** (on another partition, mounted VHD, or the running C:\Windows) without reinstalling . **Pricing**: **free for personal use** with basic component removal, unattended setup, and update/driver integration; **paid licenses unlock removal of any component**, MSIX/Appx cleanup, scheduled task management, and hardware-targeted driver import . **Home €40, Pro €90, Business €250** . **Best for IT admins and power users** wanting a polished GUI for serious image surgery, accepting that "basic" vs "any component" removal is the paywall line.

- **[Ghost Spectre](https://ghostspectre.the-ninja.jp/)**  
  **Pre-built custom Windows ISOs** distributed by an anonymous developer. **Trade-offs**: **you are trusting a pre-modified image from an unverifiable source** — no source code, no build scripts, no way to audit what was removed or added. **Widely used but carries real supply-chain risk** . **Best avoided** in favor of tools where you control the build.

## Open-Source GitHub Projects

- **[Tiny11 Builder](https://github.com/ntdevlabs/tiny11builder)**  
  **The most prominent open-source Windows 11 image slimmer** — **redesigned in WinUI 3** (v0.1.2, October 2026) with a modern interface replacing the older PowerShell-wrapper workflow . **Three modes**: **Tiny11** (removes bundled apps while keeping Windows Update, Defender, and recovery serviceable) ; **Tiny11 Core** (removes WinSxS, disables Defender and Windows Update, strips WinRE — **explicitly not suitable for regular use** and recommended only for VM testbeds) ; and **Custom** (pick exactly which apps, services, features, and components to keep or remove) . **Also adds retry logic for transient Microsoft download resets**, Cyrillic edition name decoding, and component cleanup ordering fixes in 0.1.2 . **Windows 11 26H2 support** (build 26300) with a shared servicing-base fix for 26100/26200/26300 . **Risks**: **removing protected components can break Windows** — always test your ISO in a VM first . **Best for users wanting a free, scriptable, auditable path** to a lean Windows 11 ISO with clear documentation of what each mode removes.

- **[Dism++](https://github.com/Chuyu-Team/Dism-Multi-language)**  
  **The Swiss-army GUI for DISM and system maintenance** — a frontend for Microsoft's DISM command-line utility that goes far beyond image management . **Image tools**: mount and edit WIM/ESD images, convert ESD to ISO or WIM, pack folders into ISOs, and create WIM system backups . **System optimization**: tweaks for Explorer, Windows Update, network, services, and more . **Management**: drivers, application associations, Windows features and updates, startup items, and user accounts . **Portable and free** — no installation required. **Best for users wanting a free, portable GUI** for both image editing and post-install system cleanup, without NTLite's paywall on component removal.

- **[Winaero Tweaker](https://winaero.com/)**  
  **The comprehensive freeware system tweaker** — **v1.64.2** (December 2025) adds **taskbar colorization in light mode** (a Windows 11 limitation the native Settings app doesn't support) and updates window border behavior to require sign-out for changes . **v1.64** added **dark mode**, **custom font scaling** for HiDPI displays, **drive icon customization**, and fixed **Disable Copilot**, **Disable Defender** status reporting on 24H2/25H2, and **Disable Lock screen** . **Tweaks span context menus, Explorer, Windows Update, Defender, and more** . **Note**: **taskbar customization on Windows 11 25H2 and later may be permanently limited** — ExplorerPatcher's classic taskbar restoration may no longer be viable . **Best for post-install tweaking** of a running system without touching the installation image.

- **[Optimizer-W11](https://github.com/Simv135/Optimizer-W11)**  
  **A focused Windows 11 performance and privacy tuning tool** — **creates a restore point automatically** before changes . **Optimizations span system cleanup, power tweaks, gaming priority, DirectX and MMCSS configuration, telemetry disabling, network TCP/IP tuning, USB optimization, and removal of preinstalled apps, Game Bar, and Cortana** . **Configures Windows services** including DiagTrack, Xbox, and BITS . **Simple GUI with real-time progress tracking** . **Warning**: some optimizations disable features some users prefer to keep — review before applying. **Best for gamers and power users** wanting one-click privacy and performance tuning on a running system.

- **[WinOptimizer](https://github.com/NishantJLU/Windows-Optimizer)**  
  **A safety-first modular Windows optimizer** — **automatically creates a System Restore Point** and **exports a .reg backup for every single change**, with a **Restore Center** to undo specific tweaks . **Data-driven**: bloatware lists, registry tweaks, and dev apps live in `config.json` rather than PowerShell code . **User profiles**: **Gamer** (removes Xbox services, social bloat, optimizes power), **Developer** (installs Git, Node.js, Python, VS Code, Docker via Winget), **Minimalist** (light touch), and **Manual** . **Full logging** with timestamps and status reports . **Best for users wanting transparent, reversible optimization** with per-change backups rather than a black-box "optimize" button.

- **[Rufus](https://rufus.ie/)**  
  **The standard free bootable USB creator** — GPLv3 licensed, portable, **~1.9 MB** . **Creates bootable USB media from Windows, Linux, UEFI, and other ISOs** . **Supports both BIOS and UEFI**, and can check the USB drive for bad blocks before writing . **Windows User Experience (WUE) options** add **quality-of-life toggles to disable Teams, Outlook, Copilot, and other Microsoft forced nuisances** during Windows installation, plus **silent installation** that auto-installs to the first detected disk without prompts . **Version 4.15** (June 2026) adds RISC-V 64 support to UEFI:NTFS, improved guards for the "silent" installation option, and fixes for unrestricted XML entity expansion . **Note**: **Rufus modifies the ISO's install behavior** — the WUE options are baked into the media, not applied post-install . **Best for creating Windows installation media** with optional bloat-reduction toggles at install time.

- **[Ventoy](https://www.ventoy.net/)**  
  **The multi-boot USB solution** — **100% open source** . **Copy ISO/WIM/IMG/VHD(x)/EFI files directly to the USB drive and boot them without reformatting** — no repeated flashing, just drag and drop . **Boot menu at startup** lets you choose from multiple images, organized in subdirectories . **Supports 1300+ tested ISO files** including Windows, WinPE, Linux, ChromeOS, and Unix . **UEFI Secure Boot supported**, and the drive remains usable for normal file storage . **MBR and GPT partition styles** supported, with Linux persistence and Windows/Linux auto-installation . **Best for IT admins and enthusiasts** who need a single USB stick carrying multiple OS installers and rescue tools.

### Additional Strong Open-Source Options

- **Winhance** — Open-source Windows optimization app with a major WinUI 3 upgrade (v26.02.13). **Creates a System Restore point automatically**, includes a batch uninstaller for default Microsoft apps, and can generate an Autounattend XML file for replicating settings across multiple PCs . Free via GitHub, full source code available .
- **WinOptimizer (Dunttus)** — All-in-one Python/Tkinter suite combining a WinGet GUI package manager, bloatware uninstaller, hardware telemetry, performance tweaks, system cleaner, startup manager, service manager, and Windows repair tools. GPL-3.0 licensed .
- **MSFN** — Long-running forum community (not a tool) with guides and scripts for unattended Windows setup and slipstreaming. The knowledge base is the asset, not a downloadable application.

**Frameworks for building custom systems**: Combine **Tiny11 Builder** for a free, auditable Windows 11 ISO slimming with clear mode documentation, **NTLite Free** for GUI-driven driver/update slipstreaming and unattended setup, **Dism++** for portable image editing and post-install cleanup, **Winaero Tweaker** for fine-grained UI and privacy tweaks on a running system, **Optimizer-W11** or **WinOptimizer** for automated performance and telemetry tuning with restore points, and **Ventoy** or **Rufus** for bootable media — to build a Windows customization stack where you control every change rather than trusting an anonymous pre-built ISO.

## How to Contribute

1. Fork the repo.
2. Add/edit entries in `README.md` (follow existing format).
3. Include: name, link, 1–2 sentence description, and whether it's SaaS or open-source.
4. Submit PR with a short explanation.

Star the repo if you find it useful!

## Disclaimer

- This is a **community-curated** list — not exhaustive and not an endorsement.
- OS customization tools differ significantly in risk profile, auditability, and licensing. **Ghost Spectre and other pre-built custom ISOs carry unverifiable supply-chain risk** — you cannot audit what was removed, modified, or added . **Tiny11 Core removes Windows Update, Defender, and WinRE** — it is **not suitable for regular use** and should only run in VM testbeds . **Removing protected Windows components can break the OS** — always test customized ISOs in a virtual machine before installing on primary hardware . **NTLite's free tier limits component removal to "basic" items**; removing arbitrary components requires a paid license . **Winaero Tweaker's taskbar customization on Windows 11 25H2+ may be permanently limited** . **Rufus's WUE options modify install behavior at the media level** — the toggles are baked in, not reversible post-install . **Always verify checksums** when downloading community builds, and prefer tools where you can read the scripts or source.
- Self-hosted open-source solutions require a Windows host for image building, a VM for testing customized ISOs before deployment, and awareness that slimming an image can remove functionality you later need.

---

**Made for power users, IT admins, and enthusiasts building lean Windows installations they can audit and control.**
Let's make OS customization more open, auditable, and reversible.

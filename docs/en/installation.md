# Installation

[← Documentation](./) · [Português](../pt-BR/installation.md)

Downloads are at **[zerodashboard.com.br/#downloads](https://zerodashboard.com.br/#downloads)**.
There is nothing else to install — no Python runtime, no extra packages.

---

## Windows

1. Download **`ZeroDashboard-Setup-x64.exe`**.
2. Run it. The installer offers English and Portuguese.
3. By default it installs to Program Files and asks for administrator rights.
   If you don't have them, the installer lets you install for your user only.

### The blue SmartScreen warning

On first run Windows may show **"Windows protected your PC"**. This is expected
and it is not a virus warning.

SmartScreen flags any installer that isn't signed with a paid commercial
code-signing certificate (EV Code Signing). Zero Dashboard is an independent
project and does not have one yet — the certificate costs hundreds of dollars
a year, and it is one of the things donations go toward.

To continue:

1. Click **More info** on the blue screen.
2. Click **Run anyway**.

If you'd rather verify the file first, see [Checking the download](#checking-the-download)
below.

---

## Linux

The Linux build is compiled on **Rocky Linux 9**, which sets the minimum
system library version. It runs on:

| Distribution | Minimum version |
|---|---|
| RHEL, Rocky, AlmaLinux | 9 |
| Ubuntu | 22.04 LTS |
| Debian | 12 (Bookworm) |
| Fedora | 36 |
| openSUSE Leap | 15.4 |

Older distributions are not supported — the binary requires glibc 2.34 or
newer, and there is no way around that short of a separate build.

### Debian, Ubuntu, Mint, Pop!_OS

```bash
sudo dpkg -i zerodashboard_amd64.deb
```

### Fedora, RHEL, Rocky, AlmaLinux, openSUSE

```bash
sudo rpm -i zerodashboard-x86_64.rpm
```

### AppImage (any distribution)

No installation, no root:

```bash
chmod +x ZeroDashboard-x86_64.AppImage
./ZeroDashboard-x86_64.AppImage
```

---

## Checking the download

Every release publishes a **`SHA256SUMS.txt`** next to the installers. It lets
you confirm the file arrived intact.

```bash
# Linux
sha256sum -c SHA256SUMS.txt --ignore-missing
```

```powershell
# Windows PowerShell
Get-FileHash .\ZeroDashboard-Setup-x64.exe -Algorithm SHA256
```

Compare the result with the matching line in `SHA256SUMS.txt`.

> A checksum published on the same site as the file proves the download was not
> corrupted in transit. It is not a signature, and it does not prove authorship
> — only a code-signing certificate would do that.

---

## Uninstalling

- **Windows** — Settings → Apps → Zero Dashboard → Uninstall.
- **Debian/Ubuntu** — `sudo dpkg -r zerodashboard`
- **RHEL/Fedora** — `sudo rpm -e zerodashboard`
- **AppImage** — delete the file.

Your settings live in `~/.config/oci_gui/` (`%USERPROFILE%\.config\oci_gui\` on
Windows) and are not removed by the uninstaller. Delete that folder by hand if
you want a clean slate. Your cloud credential files — `~/.aws`, `~/.oci`,
`~/.azure` — belong to the respective CLIs and are never touched.

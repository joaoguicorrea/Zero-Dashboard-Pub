# Changelog

[Português](CHANGELOG.pt-BR.md)

All notable changes to Zero Dashboard, newest first.
Downloads for the current version are at
[zerodashboard.com.br](https://zerodashboard.com.br/#downloads).

---

## v0.5.0 — 9 September 2026

**The interface is now available in English.**

- The application opens in **English (US)** by default, with a flag in the
  top-left corner to switch to **Portuguese (Brazil)**. The choice is
  remembered between runs.
- Every screen is translated — tabs, dialogs, sub-dialogs, table column
  headers, status messages and the error messages that come from the cloud
  providers.
- The Windows installer shows the licence in English when it runs in English.

**Fixes**

- Three sub-tabs (Azure Virtual WAN, Azure networking and AWS networking)
  failed to open when the interface was in English.
- The "all logs" filter in Search Logs silently stopped filtering.

## v0.4.1 — 5 September 2026

**Fixes the Linux build, which would not start.**

v0.4.0 shipped with a broken Linux binary — it opened and died immediately.
Linux is now compiled inside a Rocky Linux 9 container, with matching
components.

As a side effect the minimum glibc dropped from 2.35 to 2.34, so the `.rpm`
now runs on **RHEL, Rocky and AlmaLinux 9** — which it never did before. The
same binary was tested on Rocky 9, Ubuntu 22.04, Debian 12 and Ubuntu 24.04.

No changes to the application itself.

## v0.4.0 — 5 September 2026

**AWS and Azure fully covered, and the app is free.**

- **AWS** — SSO sign-in from inside the app, without depending on the AWS CLI;
  creating and listing profiles from the interface (SSO portal, access key,
  assume role); creating an EC2 instance, with key download on Linux and
  Administrator password on Windows; NAT gateways, VPN, Transit Gateway
  attachments, Direct Connect, and details and editing for VPC and subnet.
- **Azure** — browser sign-in without the Azure CLI; creating a VM; VNet and
  NSG editing; instance details showing private IP, public IP and the governing
  security group; Virtual WAN.
- **Licensing** — the commercial plan was dropped. The app is free, with a
  voluntary donation option.
- **Startup time** went from about 35 seconds to roughly 1 second.

## v0.3.0 — 4 September 2026

- Creating an OCI instance from the Compute tab, with an independent network
  compartment and an optional fixed private IP, validated for availability.
- Creating and deleting folders in buckets.
- Volume Groups now show the name and capacity of each volume.
- IPSec Connections and LPGs with details and editing.
- Fixed the Status filter in Search Logs, which never returned results.
- Interface aligned with the website's visual identity.

## v0.2.1 — 3 September 2026

Distribution stopped being a loose zip:

- **Windows** — a proper installer (Inno Setup) with licence screen, Start Menu
  entry, shortcut, uninstaller and silent-install support.
- **Linux** — single-file AppImage, no root required.
- The app's own icon on the executable, installer and shortcuts.

No functional changes from v0.2.0.

## v0.2.0 — 19 August 2026

- Full **AWS** administration (EC2, S3, VPC, ELB, Route 53, Elastic IP, NAT,
  Direct Connect).
- Full **Azure** administration (VMs, Blob, VNet, Load Balancer, Application
  Gateway, DNS, Public IP).
- IP and CIDR validation in firewall rules.
- An automated test suite covering the service layer.

> This release also included a NetBox integration, which was **removed in a
> later version** and is not part of the current application.

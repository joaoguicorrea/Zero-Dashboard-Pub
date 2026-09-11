# Feature inventory

[← Documentation](./) · [Português](../pt-BR/features.md)

What the application actually does, tab by tab. This is the detailed list; the
[README](../../README.md) has the summary.

Everything here is present in the shipping build. Where an operation is
destructive, the app asks for confirmation and states the consequence before
doing it.

---

## Connection

The starting point. Zero Dashboard reads the cloud profiles already configured
on your machine and lets you switch between accounts and regions without
signing in again.

- **AWS** — profiles from `~/.aws/config` and `~/.aws/credentials`, the same
  files the AWS CLI uses. Profiles created in the app work in the CLI too, and
  the other way around. Three ways to create one:
  - **SSO portal** — device-code sign-in through the browser, with account and
    permission-set selection. No AWS CLI required.
  - **Access key** — a plain access key, or a temporary one with a session token.
  - **Assume role** — borrows a source profile's identity to assume a role.
- **Azure** — browser sign-in (device code) or service principal. The
  browser method does not depend on the Azure CLI being installed.
- **OCI** — reads `~/.oci/config`.
- **Cloudflare** — API token, stored locally.

Sensitive values (ARN, OCIDs, fingerprint, API token) are masked in the
interface, with a toggle to reveal them.

## Activity

A central panel for anything that takes time or needs following up:

- **Downloads in progress**, with a byte-level progress bar.
- **Archive Tier restores** being submitted, in parallel.
- **Restores waiting for confirmation**, with reminders at 15, 30, 45 and
  60 minutes — an OCI Archive restore takes about an hour, and the app reminds
  you to go back and check instead of leaving you guessing.
- **History** of completed operations, kept between runs for 30 days. Entries
  can carry **downloadable artifacts** — for example, the CSV listing which
  IPs failed during a bulk Cloudflare operation.

---

## OCI

### Compute
List instances, lifecycle actions (start, stop, reboot), full instance details,
tag editing, and **Flex shape resizing** with the list of shapes available in
the compartment. Creating an instance covers image, shape, OCPU and memory for
Flex shapes, network compartment, VCN and subnet, private IP (next free or a
specific one, with validation against the subnet), public IP and boot volume.

### VCN / Networking
Subnets, security lists, NSGs and route tables, with rule editing on all of
them.

### Storage
Block volumes, boot volumes and volume groups; backups, restore from backup,
and attaching a volume to a VM. For buckets: an object browser, folder
creation and deletion, pre-authenticated requests (PARs), **recursive folder
download**, and total size by prefix.

**Archive Tier restore** runs in parallel and reports progress — this is the
operation the Activity panel then tracks to completion.

### VPN / DRG
DRGs, CPEs, IPSec connections with tunnel state shown by colour, tunnel detail
and editing, and local peering gateways (details and editing). The CPE
configuration file for the device at the other end can be downloaded.

### Search Logs
Queries against OCI Logging Search, with saved searches, presets for Email
Delivery, date ranges, and CSV or JSON export.

### Email Delivery
Approved senders, domains and their DKIMs, and metrics broken down by date,
sender and domain.

### Suppression List
Listing, adding and removing suppressions, with a wildcard filter that accepts
`*` anywhere in the term.

---

## AWS

### EC2
Instances with lifecycle actions, details, and instance creation (AMI, type,
subnet, key pair, security groups, private IP, volume). For Windows instances,
retrieving the Administrator password using the key pair's private key — the
key is used in memory and never stored.

### S3
Buckets, object listing and download.

### VPC / Networking
VPCs and subnets (details and editing), security groups with rule editing,
route tables, network ACLs, internet gateways, NAT gateways, VPC endpoints,
**Transit Gateways** with attachments, **VPN** (customer gateways, VPN
gateways, connections, and the router configuration file), and **Direct
Connect** with virtual interfaces, including the routes advertised on the VIF.

### Load Balancers
Application and Network Load Balancers, with listeners and target groups.

### Route 53
Hosted zones and records, with creation, editing and deletion.

### Elastic IPs
Allocate, associate with an instance, disassociate and release.

---

## Azure

### Virtual Machines
Listing with lifecycle actions (start, stop, deallocate, restart). Details show
the **private IP, the public IP and which NSG governs the machine** — including
the effective rules, which Azure computes by merging the NIC's NSG with the
subnet's. Creating a VM covers resource group, size, image, disk, network and
credentials — SSH key for Linux, user and password for Windows.

### Blob Storage
Storage accounts, containers, blob listing and download.

### VNet / Networking
VNets and subnets with details and editing, NSGs with rule editing, and route
tables. Subnet details include free-versus-usable IP counts and any delegation.

### Virtual WAN
Virtual WANs, hubs, VNet connections, site-to-site VPN, VPN sites,
ExpressRoute, route tables, and **effective routes** — the equivalent of the
portal's "Effective Routes", which is the only place propagation is visible.

### Load Balancer, Application Gateway, DNS and Public IPs
Load balancer details with frontend IPs and backend pools; Application Gateway
with start and stop; DNS zones and records; public IP listing.

---

## Cloudflare

- **Firewall Events** — with filters and a summary of top IPs and actions.
- **Zones** and **DNS Records**, with creation, editing and deletion.
- **WAF Custom Rules** — creation, editing, reordering and removal.
- **IP Access Rules**, including **bulk entry**: paste many IPs at once,
  choose the action and whether it applies to one zone or the whole account.
  The requests run in parallel, and any failures come back as a downloadable
  CSV in the Activity panel.
- **Tunnels** and their public hostnames.
- **Access Apps** and their policies.
- **Zero Trust users**, including session revocation.

---

## Across the whole application

- **Two languages** — English (US) and Portuguese (Brazil), switched by the
  flag in the corner.
- **Sortable tables** — click a column header to sort; copy a cell, a row or
  the whole table.
- **CSV export** with a UTF-8 BOM, so accented text opens correctly in Excel.
- **Confirmation before anything destructive**, stating what the consequence
  will be — not just "are you sure?".
- **Open in the provider's console** — when you need the official console for
  something the app does not cover, it takes you straight to that resource.

# Privacy and data handling

[← Documentation](./) · [Português](../pt-BR/security.md)

Zero Dashboard runs entirely on your machine and talks directly to the cloud
providers' APIs. This page describes exactly what it reads, what it writes and
what leaves your computer — so you can verify the claim rather than take it on
faith.

---

## What leaves your machine

**Requests to the cloud providers, and nothing else.**

- Calls go to AWS, Azure, OCI and Cloudflare over their official SDKs and
  HTTPS APIs, authenticated with your own credentials.
- There is **no analytics, no telemetry, no crash reporting, no update check**
  and no "phone home" of any kind.
- There is **no server in between**. The project operates a website, and the
  website only serves the installers and this documentation — the application
  never contacts it.
- The app opens **no network port**. Nothing can connect to it.

The only time a browser opens is when you ask for it: a sign-in flow, or the
"Open in console" button.

## What it reads

| Path | Why |
|---|---|
| `~/.aws/config`, `~/.aws/credentials` | AWS profiles — the same files the AWS CLI uses |
| `~/.oci/config` and the key file it points to | OCI authentication |
| Azure CLI sign-in cache | Azure authentication, when you use that method |
| `~/.config/oci_gui/` | The app's own settings |

It reads these files; it does not send them anywhere.

## What it writes

Everything the app stores lives in **`~/.config/oci_gui/`**
(`%USERPROFILE%\.config\oci_gui\` on Windows):

| File | Contents |
|---|---|
| `app.ini` | Interface preferences, such as the chosen language |
| `cloudflare.ini` | Your Cloudflare API token |
| `azure_profiles.ini` | Azure service principal profiles |
| `aws.ini` | Last profile and region used |
| `activity_log.json` | History of completed operations, kept 30 days |

Files that hold a secret are created with **owner-only permissions (`0600`)**
from the moment they exist — the app sets a restrictive umask before creating
them, rather than relaxing permissions afterwards.

When you create an AWS profile through the app, it is written to `~/.aws/` in
the standard format, so the AWS CLI sees it too.

## What it never stores

- **Private keys.** When retrieving a Windows Administrator password on EC2,
  the key pair's private key is used in memory and discarded. It is never
  written to disk by the app.
- **Passwords you type** for a new Azure VM. They go to the Azure API and are
  not kept locally.
- **Your cloud credentials**, beyond the profile files described above — which
  are the provider CLIs' own formats, in the provider CLIs' own locations.

---

## Permissions the app needs

Zero Dashboard can only do what your credential allows. It requests nothing
beyond the API calls behind the screen you are looking at.

If you want to scope a credential tightly, grant read access to the services
you intend to browse and write access only where you intend to make changes.
A read-only profile produces a read-only panel, and that is a legitimate way
to run it — a good number of the tabs are for inspection, not change.

## Destructive operations

The app can delete and modify real infrastructure, because that is what it is
for. Every destructive action asks for confirmation first, and the confirmation
states the consequence rather than just asking whether you are sure — for
example, that deleting a NAT gateway means instances in private subnets lose
their route out, and that a new one will come up with a different IP.

## Code signing

The Windows installer is **not signed** with a commercial code-signing
certificate, which is why SmartScreen shows a warning on first run. See
[Installation](installation.md#the-blue-smartscreen-warning).

Each release publishes a `SHA256SUMS.txt` next to the installers. That confirms
the download arrived intact; it is not a signature and does not prove
authorship.

## Reporting a security issue

Found something? Please email **contato@zerodashboard.com.br** rather than
opening a public issue, so it can be fixed before it is described in public.

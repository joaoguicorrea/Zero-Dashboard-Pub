# Setting up cloud profiles

[← Documentation](./) · [Português](../pt-BR/cloud-profiles.md)

Zero Dashboard does not store cloud credentials of its own. It reads the same
configuration files the official CLIs use, so anything you already have set up
works immediately — and anything you create in the app works in the CLI too.

| Provider | Where it reads from |
|---|---|
| AWS | `~/.aws/config` and `~/.aws/credentials` |
| Azure | Azure CLI sign-in, or a service principal you register in the app |
| OCI | `~/.oci/config` |
| Cloudflare | An API token you paste into the app |

On Windows, `~` is `%USERPROFILE%`.

---

## AWS

The app can create profiles for you — **Connection → AWS → Profiles…** — in
three ways.

### Through the SSO portal (recommended for company accounts)

If your company uses AWS IAM Identity Center, this is the one to use. You need
the **access portal URL**, which looks like
`https://d-xxxxxxxxxx.awsapps.com/start` and is in the invitation your company
sent you.

The app opens the authorization page, shows the code, and after you approve it
lists every account and permission set you have access to. Pick the ones you
want and it writes the profiles. No AWS CLI required.

### With an access key

Paste the access key and secret key. A session token is only needed for
temporary (STS) credentials.

The key goes to `~/.aws/credentials` with `0600` permissions and the region to
`~/.aws/config` — the same split the AWS CLI makes.

### Assuming a role

Point at a source profile that holds real credentials and give the role ARN.
The source profile can itself be an SSO profile, in which case its sign-in
covers both.

### Doing it by hand

```ini
# ~/.aws/credentials
[production]
aws_access_key_id = AKIA...
aws_secret_access_key = ...

# ~/.aws/config
[profile production]
region = us-east-1
```

📖 [AWS CLI configuration documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)

---

## Azure

Two options, both under **Connection → Azure**.

### Browser sign-in (device code)

The app shows a code, you approve it in the browser, and the sign-in is stored
on the machine until it expires. **This does not require the Azure CLI.**

Fill in the Tenant ID if you are a guest in more than one tenant — without it
the sign-in lands on your default tenant and may not see the subscription you
are looking for.

### Service principal

For automation or when browser sign-in isn't an option: tenant ID, client ID
and client secret. A service principal profile authenticates on its own and
never opens a browser.

📖 [Azure CLI sign-in documentation](https://learn.microsoft.com/cli/azure/authenticate-azure-cli)

---

## OCI

Zero Dashboard reads `~/.oci/config`. If you don't have one, the fastest route
is the OCI CLI:

```bash
oci setup config
```

Or write it by hand:

```ini
[DEFAULT]
user=ocid1.user.oc1..aaaa...
fingerprint=aa:bb:cc:...
key_file=~/.oci/oci_api_key.pem
tenancy=ocid1.tenancy.oc1..aaaa...
region=sa-saopaulo-1
```

The private key file must be readable only by you:

```bash
chmod 600 ~/.oci/oci_api_key.pem
```

📖 [OCI SDK and CLI configuration](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/sdkconfig.htm)

---

## Cloudflare

Create an API token in the Cloudflare dashboard
(**My Profile → API Tokens → Create Token**) and paste it into
**Connection → Cloudflare**.

Give it only the permissions you need. For the tabs the app offers, that is
typically:

| To use | Token needs |
|---|---|
| Zones and DNS Records | `Zone:Read`, `DNS:Edit` |
| Firewall Events and IP Access Rules | `Zone:Read`, `Firewall Services:Edit` |
| WAF Custom Rules | `Zone:Read`, `Zone WAF:Edit` |
| Tunnels and Access | `Account:Cloudflare Tunnel:Edit`, `Access: Apps and Policies:Edit` |

The token is stored at `~/.config/oci_gui/cloudflare.ini` with `0600`
permissions, and is masked in the interface.

📖 [Cloudflare API token documentation](https://developers.cloudflare.com/fundamentals/api/get-started/create-token/)

---

## Good practice

- **Scope the permissions.** Zero Dashboard can only do what the credential
  allows. A read-only profile gives you a read-only panel, which is a perfectly
  reasonable way to use it.
- **Keep credential files at `0600`.** The app does this for what it writes;
  files written by the CLIs are the CLIs' responsibility.
- **Prefer temporary credentials.** SSO and device-code sign-ins expire on
  their own; a long-lived access key does not.

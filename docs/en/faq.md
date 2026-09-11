# Frequently asked questions

[← Documentation](./) · [Português](../pt-BR/faq.md)

### Is it really free?

Yes. No cost, no trial period, no activation key, no locked features. It is
free today and stays free.

### Is it open source?

No. Free and open source are different things: Zero Dashboard costs nothing,
but the rights to it remain with the author and the source is not published.
You may pass on the original installer, unmodified and free of charge.
See the [licence](../../LICENSE.en.txt).

### Why does Windows show a blue warning?

Because the installer isn't signed with a paid commercial code-signing
certificate. It costs hundreds of dollars a year, and it is one of the things
donations go toward. The steps to continue are in
[Installation](installation.md#the-blue-smartscreen-warning).

### Does it send my data anywhere?

No. No telemetry, no analytics, no update check. Your credentials stay on your
machine and every call goes straight to the cloud provider. The details are in
[Privacy and data handling](security.md).

### Do I need the AWS CLI or the Azure CLI installed?

No. The app can sign in to both on its own, through the browser. The CLIs are
supported if you already use them — the app reads the same profile files — but
they are not a requirement.

### Do I have to create an account?

No. There is no Zero Dashboard account. Authentication is entirely against your
own cloud providers.

### Does it work on macOS?

Not at the moment. Windows and Linux only.

### Which Linux distributions are supported?

Anything with glibc 2.34 or newer: RHEL/Rocky/Alma 9, Ubuntu 22.04+, Debian 12+,
Fedora 36+, openSUSE Leap 15.4+. The table is in
[Installation](installation.md#linux).

### Can I use it with read-only credentials?

Yes, and it is a reasonable way to run it. The app can only do what your
credential allows; with read-only access you get a read-only panel. A good part
of the application is for inspection rather than change.

### Can I change the interface language?

Yes. English (US) and Portuguese (Brazil), switched with the flag in the
top-left corner. English is the default. The choice is remembered, and screens
already open keep the previous language until you reopen the app.

### Does it support more than one account per provider?

Yes. Profiles are switched from the Connection tab, and each provider keeps its
own selection — you can have AWS on one account while OCI is on another.

### Something is broken, or I have an idea

Open an issue on this repository, or email
[contato@zerodashboard.com.br](mailto:contato@zerodashboard.com.br).
For a security problem, please use email rather than a public issue.

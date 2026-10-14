# Installing Tailscale

**The one-sentence version:** One install per device — a command
on Linux, an installer on Windows/Mac, an app on phones — then
every device signs into the same account.

Install it on every machine you want in your tailnet: your home
server, your laptop, your phone, the Pi in the closet. The
per-platform notes below say what was verified and what comes
from the official docs.

## Linux

The official one-liner (verified: fetched and run in this build
environment — it detected the distro, added Tailscale's apt
repository, and installed the client; `tailscale version` below
is the real output from that install):

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

Then start the service and bring Tailscale up:

```bash
sudo systemctl enable --now tailscaled
sudo tailscale up
```

**Check:** the client is really installed and runs:

```bash
tailscale version
```

Verified output on the real client (your version number will
differ — don't expect these exact lines):

```text
1.104.1
  tailscale commit: 7f4efe814ff14b064eb080344389ceb9ea2d37ac
  long version: 1.104.1-t7f4efe814-g8da26756c
  other commit: 8da26756c9a2af700dfd05fa38d21b1eef2bcaa2
  go version: go1.27.1 (tailscale/go 24ee2fd061)
```

Before you log in, the client is honest about its state —
verified outputs:

```bash
tailscale status
```

```text
Logged out.
```

```bash
tailscale ip
```

```text
no current Tailscale IPs; state: NeedsLogin
```

That's the expected "installed but not yet joined" state. Login
is the next chapter.

Your distro may also package Tailscale itself (`apt install
tailscale`, `dnf install tailscale`, etc. — from the official
docs, not verified here). The install script is the safest
default because it always pulls the current release.

## Windows

Download the installer from Tailscale's official download page
and run it — from the official docs, not verified in this build
environment (no Windows box here). After installing, a Tailscale
icon appears in the system tray; right-click it and sign in.
Windows needs no terminal commands for the basics — `tailscale
status`, `tailscale ping`, and friends do exist in a terminal if
you want them.

## Mac

Two options, from the official docs:

- **Mac App Store:** install the Tailscale app, open it, sign
  in. Simplest path.
- **Standalone download:** from the official download page; runs
  outside the App Store sandbox with a terminal-accessible CLI.

Not verified here (no Mac in this build environment). Either way,
the post-install step is the same: sign in, and the Mac joins
your tailnet.

## iPhone and Android

Install **Tailscale** from the App Store (iOS) or Google Play
(Android) — from the official docs, not verified here. Open the
app, sign in with the same account as your other devices, and
flip the connect switch. Your phone is now on your tailnet: you
can reach your home server's web UIs from the phone's browser by
machine name, exactly like a laptop would.

One phone-specific note: mobile OSes aggressively sleep apps and
cut background network access. If your phone shows as
intermittently offline in the admin console, that's usually the
phone napping, not a Tailscale fault.

## After installing everywhere

Every device is currently in the "Logged out." state — installed,
running, belonging to nothing. The next chapter joins them into
one tailnet.

**Next:** [Your First Tailnet](your-first-tailnet.md)

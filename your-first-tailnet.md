# Your First Tailnet

**The one-sentence version:** Install Tailscale on two devices, log
in with the same account on both, and they're on one private
network — each with a name and an address that works from anywhere.

## Step 1: install on two devices

Pick any two of your machines — say your laptop and your home
server. Follow [Installing Tailscale](installing.md) for each.

## Step 2: log in

**Run this after you install — the login flow is real and needs
your browser.**

On Linux (terminal):

```bash
sudo tailscale up
```

This prints a URL like:

```text
To authenticate, visit:

        https://login.tailscale.com/a/XXXXXXXXXXXXXXXX
```

Open that URL in a browser and sign in with your Tailscale
account. What's actually happening: you're proving to Tailscale's
coordination server that this machine belongs to *you*, so it gets
added to *your* tailnet and nobody else's. The browser session is
how the account identity gets attached to the machine — the
terminal alone can't do it, because the machine has no idea who you
are until you say so.

On Windows and Mac, and on iPhone/Android, there's no terminal
step: you open the Tailscale app and sign in. Same result.

Repeat on the second device, using the **same account**. Two
devices, one account, one tailnet.

## Step 3: meet your tailnet

Every device on your tailnet now has:

- **A name** — usually the machine's hostname, cleaned up
  (something like `laptop` or `homeserver`). You can rename
  machines in the admin console (the web dashboard at
  tailscale.com, which manages your tailnet).
- **An address** — in the range `100.64.0.0/10` (for example,
  an address shaped like `100.x.y.z` — any specific address in
  this guide is an **example**). These addresses are only
  meaningful inside *your* tailnet; they work from anywhere
  because Tailscale routes them, not the public internet.

**Check:** on Linux, run this (works after login):

```bash
tailscale ip
```

It prints the device's tailnet address(es). Then:

```bash
tailscale status
```

This lists every machine on your tailnet — names, addresses, and
whether each connection is direct or relayed.

## Step 4: ping by name

From your laptop, ping your home server *by name*:

```bash
tailscale ping homeserver
```

(Use your server's actual machine name — whatever `tailscale
status` shows.)

**What success looks like:** a series of replies with latencies,
and — this is the interesting part — an indication of the path,
something like `direct` or `via derp`. Direct means the two
machines punched through NAT and connected peer to peer; `via
derp` means they're using a relay fallback. Both are encrypted;
direct is usually faster.

**Check:** if `tailscale ping` reaches the machine by name, your
tailnet works. DNS names, SSH, and everything else in this guide
rides on exactly this.

If the ping fails, don't guess — jump to
[Troubleshooting](troubleshooting.md), symptom "can't reach a
machine."

**Next:** [Reaching Your Machines](reaching-your-machines.md)

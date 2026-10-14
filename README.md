# Tailscale for Mere Mortals

**Tailscale** turns your scattered machines — the home server in the
spare room, your laptop, your phone, the little computer in the
closet — into one private network that works the same everywhere.
Coffee shop, hotel, your couch: every device gets a stable name and
address, and they can all reach each other directly, encrypted, with
no port forwarding and no router surgery. This guide teaches you to
set that up and to understand what it's doing under the hood.

No CS degree required. Ideas first, then real commands.

## What this guide covers

- **What a VPN is** — the "office tunnel" vs. Tailscale's mesh
- **How Tailscale works** — WireGuard, the coordination server,
  NAT traversal, DERP relays
- **Installing** — Linux, Windows, Mac, iPhone, Android
- **Your first tailnet** — log in, get a name and address, ping by
  name
- **Reaching your machines** — SSH and web UIs by name
- **Sharing** — let another person reach one machine
- **Exit nodes** — route your traffic out through your home machine
- **Access controls** — who can reach what, in plain language
- **Keys and expiry** — why machines "log themselves out," auth
  keys for headless servers
- **WireGuard underneath** — the machinery Tailscale is hiding from
  you, with an illustrated example
- **Troubleshooting** — symptom → fix for the common failures

## What it doesn't cover

Running your own coordination server (Headscale) — mentioned, not
taught. Enterprise single sign-on setups, and self-hosting
alternatives in general. This guide is about *understanding and
using* Tailscale as a person with machines to reach.

## How to use this guide

**Validation notes, honestly:** checked against the real Tailscale
client on a Linux box: `tailscale version`, `tailscale status`
(`Logged out.`), and `tailscale ip` (no-login state) were all run
and their outputs are real. The Linux install one-liner's script
was fetched and run — it correctly detected the distro and set up
Tailscale's package repository; the final `apt-get install` step
could not complete in this sandbox (its package manager was
wedged behind a proxy), so the end-to-end apt install is from the
official docs. `tailscale netcheck` could not complete here
either (the sandbox's egress proxy blocked the DERP map fetch).
Everything that requires a login is labeled **"run this after you
log in"** — outputs shown in those chapters are *described, not
captured*, and any example address is marked as an example. All
Tailscale addresses in this guide use the `100.64.0.0/10` range
Tailscale actually assigns, and every one of them is labeled
**example**. Windows/Mac/mobile install steps are from the
official docs (no such boxes in this build environment). If a
flag's existence is uncertain, the guide says "check `tailscale
<command> --help`".

## Chapters

1. [What Is a VPN?](what-is-a-vpn.md)
2. [How Tailscale Works](how-tailscale-works.md)
3. [Installing Tailscale](installing.md)
4. [Your First Tailnet](your-first-tailnet.md)
5. [Reaching Your Machines](reaching-your-machines.md)
6. [Sharing](sharing.md)
7. [Exit Nodes](exit-nodes.md)
8. [Access Controls](access-controls.md)
9. [Keys and Expiry](keys-and-expiry.md)
10. [WireGuard Underneath](wireguard-underneath.md)
11. [Troubleshooting](troubleshooting.md)
12. [Glossary](glossary.md)

*Written to the [nine-point writing standard](criteria.md) shared by
every guide in this series.*

# Reaching Your Machines

**The one-sentence version:** Once a machine is on your tailnet,
you reach it the way you always did — SSH, a browser URL, a file
share — except you use its tailnet *name* instead of its LAN
address, and it works from anywhere.

## SSH by name

On your LAN you might SSH like this:

```bash
ssh you@192.168.1.50
```

On your tailnet:

```bash
ssh you@homeserver
```

That's the whole change. The name `homeserver` resolves to the
machine's tailnet address wherever you are — your couch, a hotel,
your phone on cellular data.

**What changes vs. LAN SSH:**

| | LAN | Tailnet |
|---|---|---|
| Address you use | `192.168.1.50` (LAN-only) | `homeserver` (works anywhere) |
| Encryption on the wire | SSH's own encryption | SSH's encryption, *inside* WireGuard's encryption (double-wrapped) |
| Works away from home | No (unless you opened ports) | Yes |
| What you must have open | Nothing new | Nothing new — no port forwarding |

**What doesn't change:** SSH still asks for your password or key
the way it always did. Tailscale gets you *to* the machine; it
doesn't log you in. Your SSH keys and passwords still matter,
exactly as before.

**Check:** `ssh <your-username>@<your-machine-name>` (run this after you log
in — substitute your real machine name from `tailscale status`).
If you get a password prompt or land in a shell, it works.

## Reaching services by name

That web UI your server runs — the media dashboard, the router
page, the thing on port 8080 you can only see from home? Use the
name:

```text
http://homeserver:8080
```

Type that into a browser on your laptop at a coffee shop, and it
loads — encrypted end to end, no exposed ports, no dynamic-DNS
service, no "what's my home IP today."

This works for anything that listens on a TCP or UDP port: web
UIs, game servers, file shares, databases, the lot. The rule is
simple: **if it worked on your LAN by address, it works on your
tailnet by name.**

## A note on names

The names come from **MagicDNS**, Tailscale's built-in name
service. It's on by default, and it resolves short names
(`homeserver`) as well as full ones (something shaped like
`homeserver.tailXXXXXX.ts.net` — the `tailXXXXXX` part is unique
to your tailnet; treat it as a pattern, not a real address). If
names stop resolving, see
[Troubleshooting](troubleshooting.md), symptom "DNS names not
resolving."

## Safety note

Reaching your machines from anywhere is the point — but it also
means *any device on your tailnet* can reach them. If a laptop
gets stolen or a phone gets compromised, that device is inside
your private network. This is what
[Access Controls](access-controls.md) is for: decide which devices
may reach which machines, instead of the default where everything
reaches everything.

**Next:** [Sharing](sharing.md)

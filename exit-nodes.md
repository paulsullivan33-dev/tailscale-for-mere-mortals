# Exit Nodes

**The one-sentence version:** An exit node makes all of one
device's internet traffic leave through another of *your*
machines — so the coffee shop sees your home server's connection,
not your laptop's.

## What an exit node is

Normally, each device on your tailnet talks to other tailnet
devices directly, but its *internet* traffic (websites, apps, the
rest of the world) goes out through whatever network it's
physically on. An **exit node** changes that: you pick one of your
machines — typically the one at home — and tell another device,
"send all my internet traffic through *that* machine."

The path looks like this:

```text
before:   laptop --(encrypted tailnet)--> ... --> coffee shop wifi --> internet

after:    laptop --(encrypted tailnet)--> homeserver --> home ISP --> internet
```

Websites you visit see your *home* connection. The coffee shop's
network sees only encrypted WireGuard traffic heading to your
home server — it can't see which sites you're visiting.

## When you'd want one

- **Untrusted Wi-Fi.** Hotel, airport, coffee shop: route your
  traffic out through home instead of trusting the local
  network with your DNS lookups and traffic patterns.
- **Your home IP for something.** A service that only trusts
  your home IP address, or content that behaves differently by
  location — your traffic genuinely exits from your house.
- **Consistent filtering.** If your home network blocks ads or
  trackers at the DNS level, exit-node traffic gets the same
  treatment.

## When you wouldn't

- **It's slower.** Every byte takes a detour through your home
  machine. For browsing it's usually fine; for video calls or big
  downloads you may feel it.
- **It shifts trust, it doesn't remove it.** The exit node's
  network (your home ISP) now sees your traffic patterns instead
  of the coffee shop's. Encryption to the *websites* (HTTPS)
  still protects the content either way — nobody in the middle
  reads your banking session.
- **Your home upload speed is the ceiling.** Traffic goes up
  through your home connection and back down; most home
  connections upload much slower than they download.

## Turning it on and off

First, the machine that will *be* the exit node has to offer
itself. On that machine (run after login):

```bash
sudo tailscale up --advertise-exit-node
```

(Flags change over time — if this errors, check `tailscale up
--help`.)

Then approve it in the admin console — exit nodes need explicit
approval before anyone can use them, so a compromised machine
can't silently volunteer. This is deliberate.

Then, on the device whose traffic you want to route (your
laptop), point it at the exit node:

```bash
sudo tailscale set --exit-node=homeserver
```

(Use the exit node's real machine name. Again: `tailscale set
--help` if anything looks off.)

To stop using it and go back to the local network:

```bash
sudo tailscale set --exit-node=
```

An empty value clears it. Confirm with `tailscale status` — it
should no longer show an exit node in use.

## Safety note — the serious one

**Running an exit node routes someone else's traffic through
you.** If you let a friend use your home server as their exit
node, their browsing exits from *your* IP address. If they do
something abusive, it traces back to your house. Only offer exit
nodes to people you'd vouch for, and approve exit-node use in
the admin console deliberately — not by default for everyone.

**Next:** [Access Controls](access-controls.md)

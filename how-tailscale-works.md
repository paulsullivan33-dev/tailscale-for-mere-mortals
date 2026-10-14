# How Tailscale Works

**The one-sentence version:** Tailscale encrypts each connection
between your devices with WireGuard, and uses a coordination server
to introduce your devices to each other so they can connect
directly — it never sees your actual traffic.

Four pieces do the work. Each one is simple on its own.

## 1. WireGuard: the encryption

Every packet between your machines is encrypted with
**WireGuard**, a modern VPN protocol. You don't configure it
yourself — Tailscale generates a private key on each device and
handles the key exchange for you. The practical consequence: the
traffic is unreadable to anyone watching the network between the
devices, including your ISP, the coffee shop's router, and
Tailscale's own servers.

See [WireGuard Underneath](wireguard-underneath.md) for what
Tailscale is doing for you behind this curtain.

## 2. The coordination server: the introducer

Your devices need to find each other. Your phone knows its own
name and address, but it has no idea your home server exists — let
alone how to reach it through two layers of NAT and a
carrier-grade firewall.

Tailscale runs a **coordination server** (sometimes called the
control plane) that acts as the introducer:

- Each device registers: "I'm `laptop`, here's my public key,
  here are the addresses you might reach me at."
- When two devices want to talk, the server hands each of them
  the other's public key and candidate addresses.
- The devices then connect **directly to each other** and the
  server steps out of the way.

**What the coordination server sees:** machine names, public keys,
and the network addresses your devices report. It has to — that's
the directory it uses to introduce everyone.

**What it does *not* see:** your traffic. The private keys stay
on your devices; encryption happens device to device. The server
can't decrypt anything, because it never had the keys.

This is the core trust bargain: you're trusting Tailscale (or
whoever runs your coordination server) to introduce your devices
honestly, not to read your data. An honest introduction is all the
power it has.

## 3. NAT traversal: punching through

Most home and mobile networks sit behind **NAT** (Network Address
Translation): your router gives your devices private addresses and
presents one public address to the internet. Nobody outside can
start a conversation with a device behind NAT — which is exactly
why the old answer was port forwarding.

Tailscale gets around this with **NAT traversal** (often called
*hole punching*):

1. Both devices already have an *outgoing* connection to the
   coordination server (outgoing connections are allowed by NAT).
2. The server tells each device: "the other one is at this public
   address and port."
3. Both devices send packets to each other *simultaneously*.
4. Each NAT sees outgoing traffic first, so it lets the reply
   back in — a "hole" is punched through both firewalls at once.

This works on most home routers and mobile carriers. It doesn't
always work — some strict corporate or hotel networks block it —
which is why there's a fallback:

## 4. DERP relays: the fallback

When two devices can't connect directly, Tailscale routes their
traffic through a **DERP relay** — Designated Encrypted Relay for
Packets. Tailscale runs DERP servers around the world; they're
plain relays that forward your encrypted packets.

Two facts worth knowing about DERP:

- **Still encrypted.** DERP relays forward your packets but can't
  read them — they only see encrypted WireGuard blobs and where
  they're going.
- **Slower than direct.** Your traffic takes a detour through a
  relay server instead of the shortest path. Fine for remote
  access; noticeably slower for big file transfers.

You can check which you're using with `tailscale status` — it
shows whether each connection is `direct` or goes via `derp` (run
this after you log in; see [Troubleshooting](troubleshooting.md)
if everything is going through DERP and feels slow).

## The whole picture, in order

1. You install Tailscale on a device and log in.
2. The device generates keys, registers with the coordination
   server.
3. It asks: "who else is on my tailnet, and how do I reach them?"
4. The server answers with names, public keys, and addresses.
5. Your device tries a direct WireGuard connection (hole
   punching through NAT).
6. If that fails, it falls back to a DERP relay.
7. From then on, everything between the devices is encrypted,
   device to device.

No open ports on your router. No static IPs. No manual key
management.

**Next:** [Installing Tailscale](installing.md)

# Glossary

Every jargon term used in this guide, in one place.

**Access controls**
: Rules, managed in the admin console, stating which users or
devices may reach which machines (and ports). The default is
"everything reaches everything."

**Admin console**
: Tailscale's web dashboard for your tailnet: machine list,
renaming, sharing, access controls, auth keys, key expiry dates.
Replaces screenshots in this guide — anything described as "in
the admin console" is a real screen there, not depicted here.

**AllowedIPs**
: In WireGuard, the per-peer list of addresses a peer may use —
doubles as the routing table and the anti-spoofing check.
Tailscale manages these automatically.

**Auth key**
: A one-time secret generated in the admin console that lets a
machine join your tailnet without the browser login flow.
Meant for headless servers. Expires (commonly ~90 days);
prefer one-off keys over reusable ones.

**CGNAT range (100.64.0.0/10)**
: A block of IP addresses reserved for carrier-grade NAT that
will never appear on the public internet. Tailscale assigns
your machines addresses from this range (shaped like
`100.x.y.z`), which is why they never collide with real
internet addresses. Every specific address in this guide is an
**example**.

**Control plane / coordination server**
: Tailscale's servers that introduce your devices to each
other: they distribute names, public keys, and candidate
addresses. They never see your traffic — private keys stay on
your devices.

**DERP (Designated Encrypted Relay for Packets)**
: Tailscale's relay servers, used when two devices can't
connect directly. They forward encrypted packets without being
able to read them. Slower than a direct connection; a working
fallback, not a failure.

**Direct connection**
: Two devices talking peer-to-peer over WireGuard with no
relay in between — the preferred, fastest path. Shown as
`direct` in `tailscale status` / `tailscale ping`.

**Exit node**
: A machine on your tailnet configured to forward other
devices' internet traffic out through itself. Your laptop's
web browsing exits from the exit node's network instead of
the local one.

**Headless**
: A machine with no screen, keyboard, or browser — typically a
server you reach over SSH. Joins the tailnet with an auth key
instead of the browser login.

**Hole punching / NAT traversal**
: The technique both devices use to open a direct path
through their NATs simultaneously: each sends packets
outward, so each NAT lets the reply back in. Fails on some
strict networks, which then fall back to DERP.

**MagicDNS**
: Tailscale's built-in name service. Resolves machine names
(`homeserver`) and full names (shaped like
`homeserver.tailXXXXXX.ts.net`) to tailnet addresses, on any
network. On by default.

**Mesh VPN**
: A VPN with no central hub: every device connects directly to
every other device. Contrast with the classic "office tunnel"
hub-and-spoke VPN.

**NAT (Network Address Translation)**
: What your home router does: gives your devices private
addresses and presents one public address to the internet.
It's why nobody outside can reach your machines directly —
and why Tailscale has to punch through it.

**Node key**
: A device's credential proving its identity on the tailnet.
Expires (about every six months by default); expiry is why
machines occasionally "log themselves out."

**Peer**
: In WireGuard, another device your device knows about: a
public key plus an AllowedIPs list. Your tailnet is, at the
WireGuard level, a set of peers.

**Private key / public key**
: A matched pair from public-key cryptography. The private
key stays on the device and is never shared; the public key
is distributed freely. Data encrypted to a public key can
only be decrypted by its private key — this is how devices
prove identity and encrypt traffic.

**Subnet router**
: *Mentioned, not taught.* A Tailscale machine configured to
route traffic to an entire LAN subnet (not just itself),
so tailnet devices can reach non-Tailscale devices on that
LAN. An advanced topic beyond this guide's scope.

**Tailnet**
: Your private Tailscale network: the set of your devices
(plus anything shared with you) that can reach each other by
name and address from anywhere.

**tailscaled**
: The background service (daemon) that runs Tailscale on
Linux. Must be running for any `tailscale` command to work.

**VPN (Virtual Private Network)**
: A private network built over a public one: traffic is
encrypted in transit between the endpoints. Names the goal,
not the design — tunnels and meshes are both VPNs.

**WireGuard**
: The modern encryption protocol Tailscale is built on.
Handles the actual encrypted tunnels between devices;
Tailscale automates its keys, peers, and address lists.

**Next:** [Writing Criteria](criteria.md)

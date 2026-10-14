# What Is a VPN?

**The one-sentence version:** A VPN (Virtual Private Network) makes
traffic between computers private and protected as it crosses a
network you don't trust — like the public internet.

## The office tunnel

The classic VPN works like a **private tunnel to the office**. You
connect from home, and suddenly your laptop behaves as if it were
plugged into the office network: it can reach the office file
server, the office printers, the intranet pages. All your traffic
flows *through* that one tunnel and exits *through* the office
network.

That shape — one hub, spokes radiating out — made sense when the
"network" was one building and the users were people *visiting* it
remotely. But notice the shape's costs:

- Everything goes through the hub, even traffic that has nothing to
  do with the office.
- The hub is a single point of congestion and failure.
- Connecting *two* remote laptops to *each other* still routes
  through the office.

## The mesh version

A **mesh VPN** like Tailscale skips the hub. Every one of your
devices gets an address on your own private network — your
**tailnet** — and devices talk to each other *directly*, device to
device, no matter where they physically are. Your phone at the
coffee shop reaches your home server without passing through any
corporate headquarters.

| | Office-tunnel VPN | Mesh VPN (tailnet) |
|---|---|---|
| Shape | Hub and spokes | Every device connects to every device |
| What you reach | The office network | *Your* machines, wherever they are |
| When it helps | Working inside a company's network | Your phone ↔ your server ↔ your laptop |
| Traffic path | Everything via the hub | Direct device-to-device when possible |

Both are "VPNs" in the sense that traffic is encrypted across
untrusted networks. The difference is the *topology*: tunnel to one
place vs. a private network that follows you around.

## What a VPN is not

- **Not invisibility.** A VPN encrypts traffic *in transit*; it
  doesn't make you anonymous. The machine at the other end still
  knows who's connecting.
- **Not antivirus or a firewall.** It protects the *path*, not the
  endpoints. Malware on your laptop is still your problem.
- **Not a fixed shape.** "VPN" names the goal (a private network
  over a public one), not the design. Tunnels, meshes, relays —
  these are all VPNs.

## What Tailscale adds to the mesh

Two things traditional VPNs never gave you for free:

1. **No configuration on your routers.** Tailscale finds a path
   between your devices on its own — no port forwarding, no
   static IPs from your ISP, no firewall rules. How it manages
   that is the subject of [How Tailscale Works](how-tailscale-works.md).
2. **Names instead of numbers.** Your machines get human-readable
   names that work anywhere, so you SSH to `homeserver` instead of
   memorizing addresses.

**Next:** [How Tailscale Works](how-tailscale-works.md)

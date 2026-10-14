# WireGuard Underneath

**The one-sentence version:** WireGuard is the encryption engine
inside Tailscale — each device holds a private key, shares a
public key, and trusts a list of which addresses belong to which
peer; Tailscale automates all three so you never touch them.

## The three ideas

WireGuard's whole design fits in three concepts:

1. **Key pairs.** Every device generates a private key (stays on
   the device, never shared) and a public key (shared freely).
   Anything encrypted to a public key can only be decrypted by
   the matching private key. This is how your laptop knows it's
   really talking to your home server: only the server holds the
   private key matching the public key on file.

2. **Peers.** Each device keeps a list of peers: "these public
   keys are the other members of my network." A packet arriving
   from an unknown key is simply dropped — there's no login
   prompt, no handshake to attack, just silence. WireGuard calls
   this "cryptokey routing": identity *is* the key.

3. **AllowedIPs.** For each peer, a list of addresses that peer
   is allowed to send and receive: "packets *from* public key X
   may only claim to come from these addresses; packets *to*
   these addresses go *to* public key X." This is the routing
   table and the access check in one: your laptop's tailnet
   address is bound to your laptop's key, so nobody can spoof
   it.

That's the entire trust model. No certificates, no certificate
authorities, no usernames — keys, peers, and address lists.

## What Tailscale does for you

Raw WireGuard makes *you* do all of this by hand: generate keys
on every device, copy every public key to every other device,
write every address list, update everything when a device's
address changes. For two machines that's a chore; for ten it's a
part-time job.

Tailscale is, at its core, an automation layer over exactly these
three things:

| WireGuard concept | What you'd do by hand | What Tailscale does |
|---|---|---|
| Key pairs | Generate on each device, guard the private key | Generates on the device at `tailscale up`; private key never leaves |
| Peers | Copy every public key to every machine | Coordination server distributes public keys to the right devices |
| AllowedIPs | Write address lists per peer, update on change | Assigns `100.x.y.z` addresses, keeps every device's lists current |

Plus the parts raw WireGuard doesn't have at all: NAT traversal
to find paths between devices, DERP relays when it can't, names
via MagicDNS, and expiry/rotation of keys.

## An illustrated example (not a real config)

To make the three ideas concrete, here's what a *minimal*
two-device WireGuard setup looks like in WireGuard's own config
format. **This is illustrative — do not copy it onto your
machines.** Tailscale generates and manages the equivalent of
this automatically; you'd only ever write something like this if
you were using raw WireGuard without Tailscale.

```ini
# ILLUSTRATIVE EXAMPLE — laptop's view of the world.
# Addresses below are examples in Tailscale's 100.64.0.0/10 range.

[Interface]
# "This is me": my private key and my address.
PrivateKey = <laptop's private key — never leaves the laptop>
Address = 100.64.0.11/32        # example address

[Peer]
# "This is the home server": its public key, and the
# addresses I accept from it / send to it.
PublicKey = <home server's public key>
AllowedIPs = 100.64.0.12/32     # example address
```

And the home server holds the mirror image: *its* private key,
*its* address, and a `[Peer]` section with the *laptop's* public
key and the laptop's address.

Read it as a sentence: *"I am the holder of this private key, at
this address; I trust the holder of that public key, at that
address."* Everything WireGuard does — encryption, authentication,
routing — falls out of those three lines per peer.

Now imagine doing this for every pair of your ten devices, and
redoing it every time a phone changes networks. That's the job
Tailscale took over.

## What Tailscale is not hiding

One honest note: because Tailscale manages your keys, you're
trusting its coordination server to distribute the *right* public
keys (see [How Tailscale Works](how-tailscale-works.md)). Raw
WireGuard lets you verify keys yourself, out of band, at the
cost of doing everything by hand. Tailscale trades that manual
verification for convenience — a reasonable trade for a home
network, and one you should make with your eyes open.

**Next:** [Troubleshooting](troubleshooting.md)

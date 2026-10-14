# Access Controls

**The one-sentence version:** By default every device on your
tailnet can reach every other device; access controls let you
change that default — in plain language, a list of who may talk to
whom.

## The default: everything reaches everything

When you add machines to your tailnet, the starting rule is
simple: **any device can reach any device.** Your phone can SSH to
your home server. Your laptop can reach the Pi in the closet. That
machine you shared with a friend can reach... wait, no — shared
machines are the exception: a shared machine is visible only to
the person it was shared with, not to your whole tailnet.

For a tailnet that's just *your* devices, the default is fine and
you can stop reading here. Access controls matter when:

- You've **shared** a machine with someone (see
  [Sharing](sharing.md)) and want to limit what they can do with
  it.
- A device is **higher-risk** — a phone that travels, a laptop a
  kid uses — and you want it to reach *fewer* machines.
- You run **services** you want visible to some people but not
  others.

## How it works, in plain language

In the admin console there's an access-control policy — a
document that lists rules. The default policy is effectively one
rule:

```text
everyone  →  everything
```

You can replace it with narrower rules, for example in plain
English:

```text
everyone  →  the media server, ports for web only
friend    →  the shared machine, SSH only
your phone →  everything
```

The actual policy is written in a structured format (JSON) in the
admin console. You don't need to memorize the syntax to
understand the *idea*: each rule names **who** (a user, or a
device), **what** (a machine, possibly limited to certain ports),
and the effect is allow or deny.

**A concrete example of the thinking:** you share your home
server with a friend so they can use the media library (a web UI
on port 8096). Without access controls, they could also SSH into
the server. With a rule that says "friend → homeserver, port 8096
only," they get the media library and nothing else. That's the
whole game: narrow the permission to what the person actually
needs.

## What access controls can't do

- **They don't encrypt anything extra.** The traffic was already
  encrypted by WireGuard; access controls decide *whether the
  connection is allowed*, not how it's protected.
- **They don't protect a compromised device's own data.** If a
  laptop is compromised, the attacker can use whatever *that
  device* is allowed to reach. Access controls limit the blast
  radius; they don't shrink it to zero.
- **They're not per-file or per-app.** They work at the network
  level — which machines, which ports. Finer permissions (who can
  read which folder) still belong to the applications and the
  operating system.

## Getting started

1. Open the admin console and find the access controls section.
2. Read the current policy — confirm it's the default
   allow-everything.
3. Change **one thing** first: restrict one shared machine or one
   risky device. Test that the thing you wanted to keep working
   still works.
4. Save, and verify from the affected device.

Change one rule at a time. Locking yourself out of your own
server with an over-eager rule is a rite of passage — doing it in
small steps means you know exactly which change to undo. (If it
happens, the admin console can fix the policy; you're not locked
out of the console, just the machine.)

**Next:** [Keys and Expiry](keys-and-expiry.md)

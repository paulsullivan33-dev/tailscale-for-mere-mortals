# Sharing

**The one-sentence version:** You can let another person reach
*one* of your machines without giving them your whole tailnet —
they see that machine and nothing else.

## When you'd do this

Your friend needs to grab a file from your home server. A family
member wants to use the media library you host. A collaborator
needs to test against the web app running on your machine.

The old options were bad: hand over your Wi-Fi password (no),
open a port on your router (worse), or add their laptop to your
entire tailnet (too much — now they can reach *everything*).

## How sharing works

In the Tailscale admin console (the web dashboard for your
tailnet), you can share a single machine with another Tailscale
user. They accept the invite, and the shared machine appears in
*their* tailnet alongside their own machines.

**What they can do:**

- Reach the shared machine by its name, exactly like you do —
  SSH, web UIs, any service it runs.
- Nothing else. They can't see your other machines, your
  machine list, or your tailnet's settings.

**What they can't do:**

- See or reach any of your *other* machines.
- Change anything about your tailnet (rename machines, change
  access controls, add or remove devices).
- See traffic between your other machines.

Think of it as lending someone one key that opens one room,
while the rest of the house stays locked.

## What the other person needs

They need a Tailscale account and the Tailscale client installed
on their device — same as you did in
[Your First Tailnet](your-first-tailnet.md). You send the share
from your admin console; they accept. No passwords exchanged, no
router changes on either side.

## Safety note — read this before sharing

**Sharing exposes a real machine to a real person.** A few
plain truths:

- They can reach *every service* that machine runs — not just
  the one you had in mind. If your home server also hosts your
  file archive, your notes, and your smart-home dashboard, the
  person you shared with can reach all of those too, unless you
  restrict them with [access controls](access-controls.md).
- You're trusting them not to be careless: if *their* device is
  compromised, the attacker inherits their access to your shared
  machine.
- You can revoke the share at any time from the admin console.
  Do it when the need ends — don't leave shares open
  indefinitely.

**The rule:** share the *most specific* machine you can (a small
dedicated box beats your do-everything server), restrict it with
access controls, and revoke when done.

**Next:** [Exit Nodes](exit-nodes.md)

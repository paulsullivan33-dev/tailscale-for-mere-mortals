# Troubleshooting

**The one-sentence version:** Most Tailscale problems are one of
five things — find your symptom below and work the fix in order.

## Symptom: can't reach a machine

The ping fails, SSH hangs, the web UI won't load.

1. **Is Tailscale running on *both* ends?** On each machine,
   `tailscale status` (run after login) should list the tailnet.
   If it says you're logged out, log back in — see "logged out"
   below.
2. **Does the machine show as online?** In `tailscale status`
   or the admin console, an offline machine isn't reachable,
   full stop. See "machine shows offline" below.
3. **Can you reach it by address but not by name?** Then it's a
   DNS problem, not a connectivity problem — see "DNS names not
   resolving."
4. **Check the path.** `tailscale ping <name>` tells you
   whether the connection is `direct` or `via derp`. If even the
   ping fails entirely, something is blocking UDP — a strict
   firewall or captive portal. Try another network to confirm.
5. **Check access controls.** If you recently narrowed the
   policy, the machine may be correctly refusing you. Review the
   last rule you changed in the admin console.

## Symptom: everything goes through DERP (slow)

`tailscale status` or `tailscale ping` shows `via derp` for
connections that should be direct, and transfers feel sluggish.

- **This is a fallback, not a failure.** Traffic is still
  encrypted; it's just taking a detour through a relay. Don't
  panic — diagnose.
- **Run `tailscale netcheck`.** This reports what your network
  looks like from the outside: whether UDP is reachable, what
  kind of NAT you're behind. If it reports that UDP is blocked,
  that's your answer — the network won't allow direct
  connections.
- **Usual culprits:** strict corporate or hotel firewalls,
  "symmetric" NAT on some mobile carriers and ISP routers,
  VPN software running *underneath* Tailscale fighting over
  routes.
- **What helps:** a direct connection needs UDP to flow both
  ways. On networks you control, check that no firewall rule is
  blocking outbound UDP. On networks you don't control (hotel
  Wi-Fi), DERP is the answer and there's nothing to fix — it's
  doing its job.

## Symptom: "logged out" / key expired

`tailscale status` reports you're logged out, or a machine silently dropped off the tailnet.

- **It's almost certainly key expiry.** Node keys expire (about
  every six months by default). See
  [Keys and Expiry](keys-and-expiry.md) for why.
- **The fix:** log in again. On Linux: `sudo tailscale up` and
  follow the browser prompt. In the apps: sign back in. The
  machine keeps its name and address.
- **If it happens on a headless server:** you need console or
  SSH access on its LAN address to re-run the login — or better,
  join it with an auth key next time so no browser is needed.
- **To stop being surprised:** the admin console shows each
  machine's key expiry date. Check it when you set a machine up
  and note when it'll need attention.

## Symptom: machine shows offline

The admin console or `tailscale status` lists the machine as
offline.

1. **Is the machine actually powered on and on a network?**
   Start with the obvious — offline usually means *off* or
   *disconnected*, not a Tailscale problem.
2. **Is the Tailscale client running?** On Linux, the
   `tailscaled` service should be active. A reboot sometimes
   leaves it stopped if it wasn't enabled to start on boot.
3. **Laptop asleep?** A sleeping machine is offline. That's
   normal, not a fault.
4. **Stale entry?** If you reinstalled a machine or it got a
   new identity, the old entry may linger as offline in the
   console. Remove the stale machine entry — the fresh one is
   the real one.

## Symptom: DNS names not resolving

`tailscale ping 100.x.y.z` (an address from `tailscale status`)
works, but `tailscale ping machinename` fails.

- **MagicDNS must be enabled** in the admin console's DNS
  settings — it's on by default, but check if names ever
  worked and then stopped.
- **The resolving device must be using Tailscale's DNS.** On
  some Linux setups, the system's DNS configuration doesn't
  pick up Tailscale's resolver. `tailscale status` working by
  address but not by name is the signature of this.
- **On Linux**, if another DNS manager (like systemd-resolved
  with strict settings, or a manually-written
  `/etc/resolv.conf`) is in charge, Tailscale's names may not
  resolve. The admin console's DNS docs cover the common
  configurations — this one varies enough by distro that the
  guide won't pretend there's a single command.
- **Sanity check:** `tailscale ip` on the target confirms its
  address; if the address works and the name doesn't, it's DNS,
  not the network.

## When none of this helps

- `tailscale bugreport` (run on the affected machine) collects
  logs into a report you can share when asking for help.
- The admin console shows per-machine details — last seen,
  client version, key expiry — that often name the problem
  faster than guessing from the client side.
- And the unglamorous fix that works embarrassingly often:
  `sudo tailscale down` followed by `sudo tailscale up`
  re-establishes everything from scratch.

**Next:** [Glossary](glossary.md)

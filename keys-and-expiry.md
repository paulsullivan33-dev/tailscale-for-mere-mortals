# Keys and Expiry

**The one-sentence version:** Machines on your tailnet periodically
need to re-prove who they are — that's why a server sometimes
"logs itself out" — and auth keys let headless machines join
without a browser.

## Why machines "log themselves out"

Every device on your tailnet holds a **node key**: proof that it
is who it claims to be. Node keys **expire** — by default after
about six months (check the exact date for any machine in the
admin console; the default can be changed).

When a key expires, the machine drops off the tailnet. It looks
like the machine "logged itself out" or "forgot" Tailscale. It
didn't forget anything — its credential simply aged out, the same
way a password policy forces a rotation.

**Why expire keys at all?** Because devices get lost, sold, and
compromised. Expiry is a safety net: a laptop you lost a year ago
can't quietly rejoin your network forever. The inconvenience of
re-authenticating twice a year is the price of that guarantee.

**Re-authenticating** is just logging in again — `sudo tailscale
up` on Linux and follow the browser prompt, or sign back in in the
app. The machine keeps its name and address.

## Auth keys: for machines with no browser

Here's the problem: your home server has no screen, no browser,
nobody sitting at it. When *its* key expires, you can't walk it
through a login page — and even the *first* login is awkward
without a browser handy.

The answer is an **auth key**: a one-time secret you generate in
the admin console and hand to the machine instead of doing the
browser dance.

On the headless machine (run once, when joining it to the
tailnet):

```bash
sudo tailscale up --auth-key=tskey-auth-XXXXXXXXXXXXXXXX
```

(Substitute the real key from your admin console — never share
these; anyone holding one can join *your* tailnet as a new
machine.)

The machine joins immediately, no browser involved. You can also
paste that same command over SSH — which is the usual workflow:
SSH into the fresh server on its LAN address, run `tailscale up
--auth-key=...`, and from then on reach it by tailnet name.

## Auth keys expire too

Auth keys are secrets, and secrets have lifetimes:

- An auth key itself expires — commonly after about 90 days
  (the admin console shows the exact expiry when you create
  one). Generate a fresh one when you need it; don't stockpile.
- A key can be **reusable** (joins many machines) or
  **one-off** (joins one machine, then dies). Prefer one-off
  keys: a leaked reusable key is a standing invitation to your
  tailnet.
- A machine joined with an auth key gets its own node key with
  its own expiry — the auth key's job is done at join time.

**Treat auth keys like passwords:** generate when needed, use
once, and revoke them in the admin console when you're done.
(There's a key-management section in the admin console showing
every key you've created and whether it's been used.)

## The maintenance rhythm

Put plainly, a tailnet needs occasional tending:

| What | How often (defaults) | Action |
|---|---|---|
| Node key expiry | ~every 6 months per machine | Re-authenticate (browser or app) |
| Auth keys | expire ~90 days after creation | Generate fresh when joining a machine |
| Shared machines | whenever the need ends | Revoke the share |

None of this is daily work. But the first time a server "logs
itself out" at an inconvenient moment, you'll be glad you knew
*why* — and that the fix is one login, not a network rebuild.

**Next:** [WireGuard Underneath](wireguard-underneath.md)

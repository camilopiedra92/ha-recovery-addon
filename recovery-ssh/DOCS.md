# Recovery SSH

A root shell on the Pi for recovery. It exists because the Terminal & SSH app
starts its SSH server only when a port is published on the host, and this setup
publishes none.

## Configuration

```yaml
authorized_keys:
  - ssh-ed25519 AAAA... owner@laptop
```

Public keys only, one per entry, bare ed25519 (`ssh-ed25519` or `sk-ssh-ed25519`)
with no options in front. The app refuses to start with no key, or with an entry
that is not one of those, so a typo cannot leave it running with nobody able to
log in. There is no password login.

## Reaching it

The app listens on port 22 of its own container and publishes nothing. Its
internal hostname is shown on the app's Info page (`<hash>-recovery-ssh`).
Point something that already runs inside Home Assistant at `<hostname>:22`: a
Cloudflare Tunnel route of type `ssh://`, or a Tailscale Service with a `tcp://`
target. Put identity checks in front of it (Cloudflare Access, Tailscale ACLs):
the key is the second factor, not the only one.

## What a session can do

`ha` (the Supervisor CLI, as `manager`), `git`, and the Home Assistant
configuration read-write at `/config` (the `path` of the mapping). That is
enough to read Core's log, restore `/config` to a known commit and restart Core.

## Staying up

If sshd dies, s6 restarts it. If the container itself stops, enable the
Supervisor's watchdog for this app (Info page, or `watchdog: true` through the
Supervisor API) so it is started again; `boot: auto` only covers a reboot.

## Host key

Generated on first start and kept in the app's data, so the fingerprint is
stable across restarts. It is printed in the app log at every start.

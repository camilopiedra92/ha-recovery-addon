# ha-recovery-addon

A Home Assistant add-on that gives the owner a key-only SSH shell on the Pi with
Supervisor access (`ha`, `git`, `/config`), so Home Assistant can be recovered
when Core is down. It publishes no port: something on the Docker network (a
Cloudflare tunnel, Tailscale) has to carry the connection in. See
[recovery-ssh/DOCS.md](recovery-ssh/DOCS.md).

Install: Settings > Apps > App store > menu > Repositories > add
`https://github.com/camilopiedra92/ha-recovery-addon`.

The design and the reasons for it live in the owner's `ha` repository (feature
`002-reliable-recovery-access`), not here.

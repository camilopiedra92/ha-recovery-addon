# ha-recovery-addon

Home Assistant add-on repository: one add-on, `recovery-ssh`, a key-only SSH shell with Supervisor access for recovering the owner's Pi when Core is down. It is public on purpose (the Supervisor can only install from a repository it can clone) and must never contain a secret.

## Commands

- Checks (what CI runs): `shellcheck -s bash recovery-ssh/rootfs/etc/s6-overlay/s6-rc.d/*/run recovery-ssh/rootfs/etc/s6-overlay/s6-rc.d/sshd/finish`, hadolint on `recovery-ssh/Dockerfile`, and the Home Assistant add-on linter on `recovery-ssh/`.
- Build and try it locally (Apple silicon matches the Pi): `docker build --build-arg BUILD_ARCH=aarch64 -t recovery-ssh-test recovery-ssh`. bashio needs a Supervisor, so run sshd by hand in the container with a throwaway host key and authorized key, as the checks in the `ha` repo's quickstart describe.

## Invariants

- `config.yaml` has no `ports` and no `host_network`: nothing may be published on the host (constitution IV of the `ha` repository).
- sshd is key-only (`PasswordAuthentication no`, `PubkeyAuthentication yes`) and `PermitRootLogin prohibit-password`. A change that adds a password path breaks the feature's contract.
- The `ha` CLI download in the Dockerfile is pinned by version and sha256; bumping the version means recomputing the hash.

## Where things live

The spec, plan and tasks are in `~/Development/ha/specs/002-reliable-recovery-access/`; this repository implements the add-on part of that feature and has no Spec Kit of its own.

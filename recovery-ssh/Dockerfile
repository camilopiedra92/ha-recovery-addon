ARG BUILD_FROM=ghcr.io/hassio-addons/base:21.0.8
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

ARG BUILD_ARCH
ARG HA_CLI_VERSION=5.5.0
# sha256 of ha_aarch64 5.5.0, checked against the file downloaded on 2026-10-16.
ARG HA_CLI_SHA256=af390ab7f91a8135668ace7b1425d8e53800b6a1fc3b36fc588df701d2ab3db5

# hadolint ignore=DL3018
RUN \
    apk add --no-cache git openssh \
    \
    && curl -fsSL -o /usr/bin/ha \
        "https://github.com/home-assistant/cli/releases/download/${HA_CLI_VERSION}/ha_${BUILD_ARCH}" \
    && echo "${HA_CLI_SHA256}  /usr/bin/ha" | sha256sum -c - \
    && chmod 0755 /usr/bin/ha

COPY rootfs /

ARG BUILD_DATE
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_VERSION

LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.source="https://github.com/camilopiedra92/ha-recovery-addon" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}

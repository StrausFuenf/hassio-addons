ARG BUILD_FROM=ghcr.io/hassio-addons/debian-base:7.8.3
###############################################################################
# Get prebuild containers from Vaultwarden
###############################################################################
FROM "vaultwarden/server:1.34.1" as vaultwarden

###############################################################################
# Build the actual add-on.
###############################################################################
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Get the Bitwarden from official images
COPY --from=vaultwarden /vaultwarden /opt/vaultwarden
COPY --from=vaultwarden /web-vault /opt/web-vault

# add Nginx
# hadolint ignore=DL3009
RUN \
    apt-get update \
    \
    && apt-get install -y --no-install-recommends \
        libmariadb-dev-compat \
        libpq5 \
        nginx \
        sqlite3 \
        fail2ban \
        iproute2 \
        argon2 \
    && apt-get clean \
    && rm /etc/fail2ban/jail.d/defaults-debian.conf \
    && rm /etc/fail2ban/fail2ban.conf \
    && rm /etc/nginx/sites-enabled/default \
    && rm -f /etc/nginx/nginx.conf
# Copy root filesystem
COPY rootfs /

#!/bin/bash

RUN chmod u+rwx  /etc/s6-overlay/s6-rc.d/fail2ban/run
RUN chmod u+rwx  /etc/s6-overlay/s6-rc.d/nginx/run
# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="StrausFuenf <admin@strausfuenf.de>" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="StrausFuenf's Home Assistant Add-ons" \
    org.opencontainers.image.authors="StrausFuenf <admin@strausfuenf.de>" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://github.com/StrausFuenf/hassio-addons" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}

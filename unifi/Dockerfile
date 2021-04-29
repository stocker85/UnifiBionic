ARG BUILD_FROM=ghcr.io/hassio-addons/ubuntu-base/amd64:6.1.4
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base system
ARG BUILD_ARCH=amd64
RUN \
    URL="http://archive.ubuntu.com/ubuntu/" \
    && if [ "${BUILD_ARCH}" = "armv7" ] \
        || [ "${BUILD_ARCH}" = "aarch64" ]; then \
        URL="http://ports.ubuntu.com/ubuntu-ports/"; \
    fi \
    \
    && apt-get update \
    \
    && apt-get install -y --no-install-recommends gnupg \
    \
    && apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2930ADAE8CAF5059EE73BB4B58712A2291FA4AD5 \
    \
    && echo "deb http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.6 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-3.6.list \
    \
    && apt-get update \
    \
    && apt-get install -y --no-install-recommends \
        binutils=2.30-21ubuntu1~18.04.5 \
        jsvc=1.0.15-8 \
        libcap2=1:2.25-1.2 \
        logrotate=3.11.0-0.1ubuntu1 \
        mongodb-org=3.6.17 \
        openjdk-8-jdk-headless=8u282-b08-0ubuntu1~18.04 \
    \
    && curl -J -L -o /tmp/unifi.deb \
        "https://dl.ui.com/unifi/6.2.23-d3a253013f/unifi_sysvinit_all.deb" \ 
    \
    && dpkg --install /tmp/unifi.deb \
    \
    && rm -fr \
        /tmp/* \
        /root/.gnupg \
        /var/{cache,log}/* \
        /var/lib/apt/lists/*

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION

# Labels
LABEL \
 io.hass.name="UniFi Controller" \
    io.hass.description="Manage your UniFi network using a web browser" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Franck Nijhof <frenck@addons.community>" \
    org.opencontainers.image.title="UniFi Controller" \
    org.opencontainers.image.description="Manage your UniFi network using a web browser" \
    org.opencontainers.image.vendor="Home Assistant Community Add-ons" \
    org.opencontainers.image.authors="Franck Nijhof <frenck@addons.community>" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://addons.community" \
    org.opencontainers.image.source="https://github.com/hassio-addons/addon-unifi" \
    org.opencontainers.image.documentation="https://github.com/hassio-addons/addon-unifi/blob/master/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}

# syntax=docker/dockerfile:1
FROM centos:7

# Set shell options for safety
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Set build-time variables
ENV NESSUS_VERSION="7.0.3"
ENV NESSUS_NAME="Nessus_Docker_Container"

# Update and install dependencies
# hadolint ignore=DL3033
RUN set -x && \
    yum -y update nss-util bind-license libssh2 && \
    yum -y install expect && \
    yum clean all && \
    rm -rf /var/cache/yum

# Import Tenable's GPG key
RUN rpm --import https://static.tenable.com/marketing/RPM-GPG-KEY-Tenable

# Download Nessus
RUN DOWNLOAD_ID=$(curl -ssl -o - "https://www.tenable.com/downloads/nessus" | sed -n -e 's/.*data-download-id="\([0-9]*\)".*data-file-name="\([a-zA-Z0-9_\.-]\+\-es7\.x86_64\.rpm\).*".*/\1/p') && \
    curl -ssL -o "/tmp/Nessus-${NESSUS_VERSION}-es7.x86_64.rpm" "https://tenable-downloads-production.s3.amazonaws.com/uploads/download/file/${DOWNLOAD_ID}/Nessus-${NESSUS_VERSION}-es7.x86_64.rpm" && \
    (yum -y localinstall "/tmp/Nessus-${NESSUS_VERSION}-es7.x86_64.rpm" || \
    yum -y localinstall "/tmp/Nessus-${NESSUS_VERSION}-es7.x86_64.rpm") && \
    yum clean all && \
    rm "/tmp/Nessus-${NESSUS_VERSION}-es7.x86_64.rpm" && \
    rm -rf /var/cache/yum && \
    rm -rf /opt/nessus/var/nessus/uuid /opt/nessus/var/nessus/*.db* /opt/nessus/var/nessus/master.key

# Add NESSUS_USER and register with Tenable Cloud
# Uses BuildKit secret mount to avoid exposing NESSUS_KEY in image layers
COPY add_user.exp /
RUN expect /add_user.exp
RUN --mount=type=secret,id=nessus_key \
    NESSUS_KEY=$(cat /run/secrets/nessus_key) && \
    /opt/nessus/sbin/nessuscli managed link --key="${NESSUS_KEY}" --name="${NESSUS_NAME}" --cloud

# Create non-root user
RUN useradd -m -u 1000 nessususer
USER nessususer

# Add healthcheck
HEALTHCHECK --interval=30s --timeout=10s --start-period=60s --retries=3 \
    CMD curl -k -f https://localhost:8834/ || exit 1

EXPOSE 8834
CMD ["/opt/nessus/sbin/nessus-service"]

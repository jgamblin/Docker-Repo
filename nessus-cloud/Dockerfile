# syntax=docker/dockerfile:1
FROM debian:bookworm-slim

LABEL org.opencontainers.image.source="https://github.com/jgamblin/Docker-Repo"
LABEL org.opencontainers.image.description="Nessus vulnerability scanner linked to Tenable Cloud"
LABEL org.opencontainers.image.licenses="MIT"
LABEL org.opencontainers.image.authors="Jerry Gamblin"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

ARG NESSUS_VERSION="10.8.3"
ENV NESSUS_NAME="Nessus_Docker_Container"

# hadolint ignore=DL3008
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        expect \
    && rm -rf /var/lib/apt/lists/*

# Download and install Nessus .deb package
RUN DOWNLOAD_URL=$(curl -sL "https://www.tenable.com/downloads/nessus?loginAttempted=true" \
        | grep -oP 'https://[^"]+Nessus-'"${NESSUS_VERSION}"'-debian10_amd64\.deb' \
        | head -1) && \
    curl -sL -o /tmp/nessus.deb "${DOWNLOAD_URL}" && \
    dpkg -i /tmp/nessus.deb && \
    rm /tmp/nessus.deb && \
    rm -rf /opt/nessus/var/nessus/uuid \
           /opt/nessus/var/nessus/*.db* \
           /opt/nessus/var/nessus/master.key

# Create default admin user via expect script
COPY add_user.exp /tmp/add_user.exp
RUN expect /tmp/add_user.exp && rm /tmp/add_user.exp

# Link scanner to Tenable Cloud using BuildKit secret
# Build with: docker build --secret id=nessus_key,src=./nessus.key -t nessus-cloud .
RUN --mount=type=secret,id=nessus_key \
    NESSUS_KEY=$(cat /run/secrets/nessus_key) && \
    /opt/nessus/sbin/nessuscli managed link \
        --key="${NESSUS_KEY}" \
        --name="${NESSUS_NAME}" \
        --cloud

RUN useradd -m -u 1000 nessususer
USER nessususer

HEALTHCHECK --interval=30s --timeout=10s --start-period=60s --retries=3 \
    CMD curl -k -f https://localhost:8834/ || exit 1

EXPOSE 8834

CMD ["/opt/nessus/sbin/nessus-service"]

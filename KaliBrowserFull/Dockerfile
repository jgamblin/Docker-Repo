# checkov:skip=CKV_DOCKER_7:Kali Rolling is a rolling release with no fixed tags
# trivy:ignore:AVD-DS-0001
# hadolint ignore=DL3006
FROM kalilinux/kali-rolling

LABEL org.opencontainers.image.source="https://github.com/jgamblin/Docker-Repo"
LABEL org.opencontainers.image.description="Kali Linux with browser-based VNC access via noVNC"
LABEL org.opencontainers.image.licenses="MIT"
LABEL org.opencontainers.image.authors="Jerry Gamblin"

ENV HOME=/root \
    DEBIAN_FRONTEND=noninteractive \
    LC_ALL=C.UTF-8 \
    LANG=en_US.UTF-8 \
    LANGUAGE=en_US.UTF-8 \
    DISPLAY=:0.0

# hadolint ignore=DL3008
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        kali-linux-default \
        ca-certificates \
        curl \
        git \
        xvfb \
        x11vnc \
        xdotool \
        wget \
        unzip \
        firefox-esr \
        openbox \
        menu \
        supervisor \
        x11-apps \
        net-tools \
        lightdm \
        python3 \
        python3-numpy \
        novnc \
        websockify \
    && rm -rf /var/lib/apt/lists/*

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

RUN useradd -m -u 1000 kaliuser
USER kaliuser

HEALTHCHECK --interval=30s --timeout=5s --start-period=30s --retries=3 \
    CMD curl -f http://localhost:6080/ || exit 1

EXPOSE 6080

CMD ["/usr/bin/supervisord"]

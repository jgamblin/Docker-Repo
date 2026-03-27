FROM alpine:3.21

LABEL org.opencontainers.image.source="https://github.com/jgamblin/Docker-Repo"
LABEL org.opencontainers.image.description="Lightweight Tor SOCKS5 proxy on Alpine Linux"
LABEL org.opencontainers.image.licenses="MIT"
LABEL org.opencontainers.image.authors="Jerry Gamblin"

# hadolint ignore=DL3018
RUN echo '@testing http://nl.alpinelinux.org/alpine/edge/testing' \
    >> /etc/apk/repositories && \
    apk --update --no-cache add tor@testing runit@testing

COPY --chmod=755 service /etc/service/
COPY torrc /etc/tor/torrc

RUN adduser -D -u 1000 toruser && \
    chown -R toruser:toruser /etc/service /var/lib/tor

USER toruser

HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
    CMD pgrep tor || exit 1

CMD ["runsvdir", "/etc/service"]

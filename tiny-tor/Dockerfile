FROM alpine:3.21

RUN echo '@testing http://nl.alpinelinux.org/alpine/edge/testing' \
    >> /etc/apk/repositories && \
    apk --update --no-cache add tor@testing runit@testing

COPY service /etc/service/
COPY torrc /etc/tor/torrc

# Create non-root user for tor
RUN adduser -D -u 1000 toruser && \
    chown -R toruser:toruser /etc/service /var/lib/tor
USER toruser

# Add healthcheck
HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
    CMD pgrep tor || exit 1

CMD runsvdir /etc/service

FROM alpine:edge

RUN echo '@testing http://nl.alpinelinux.org/alpine/edge/testing' \
    >> /etc/apk/repositories && \
    apk --update add tor@testing runit@testing

COPY service /etc/service/
COPY torrc /etc/tor/torrc

CMD runsvdir /etc/service

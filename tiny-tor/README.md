# tiny-tor

Lightweight Tor SOCKS5 proxy running on Alpine Linux. Routes traffic through the Tor network via a containerized SOCKS proxy on port 9050.

## Build

```bash
docker build -t tiny-tor tiny-tor/
```

## Run

```bash
docker run -d --name tor-proxy -p 9050:9050 tiny-tor
```

Test the proxy:

```bash
curl --socks5-hostname 127.0.0.1:9050 https://check.torproject.org/api/ip
```

## Configuration

The default `torrc` exposes a SOCKS proxy on `0.0.0.0:9050`. To customize Tor settings, mount your own config:

```bash
docker run -d --name tor-proxy -p 9050:9050 -v ./my-torrc:/etc/tor/torrc tiny-tor
```

See the included `torrc` for documented options including hidden services, relay configuration, and bandwidth throttling.

## Security Notes

- Runs as non-root user `toruser` (UID 1000)
- No exit relay by default — the container only acts as a client proxy
- The SOCKS port binds to `0.0.0.0` inside the container; use Docker's port mapping (`-p 127.0.0.1:9050:9050`) to restrict access to localhost

# Docker-Repo

Security-focused Docker containers for penetration testing, vulnerability scanning, and privacy.

[![Lint Code Base](https://github.com/jgamblin/Docker-Repo/actions/workflows/SuperLinter.yml/badge.svg)](https://github.com/jgamblin/Docker-Repo/actions/workflows/SuperLinter.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

## Containers

| Container                           | Description                              | Base Image           |
| ----------------------------------- | ---------------------------------------- | -------------------- |
| [tiny-tor](tiny-tor/)               | Lightweight Tor SOCKS5 proxy             | Alpine 3.21          |
| [nessus-cloud](nessus-cloud/)       | Nessus scanner linked to Tenable Cloud   | Debian bookworm-slim |
| [KaliBrowserFull](KaliBrowserFull/) | Kali Linux with browser-based VNC access | Kali Rolling         |

## Quick Start

**Tor proxy:**

```bash
docker build -t tiny-tor tiny-tor/
docker run -d -p 9050:9050 tiny-tor
```

**Nessus scanner** (requires [Tenable Cloud](https://cloud.tenable.com) linking key):

```bash
echo "YOUR_KEY" > nessus.key
DOCKER_BUILDKIT=1 docker build --secret id=nessus_key,src=./nessus.key -t nessus-cloud nessus-cloud/
docker run -d -p 8834:8834 nessus-cloud
```

**Kali in the browser:**

```bash
docker build -t kali-browser KaliBrowserFull/
docker run -d -p 6080:6080 kali-browser
```

See each container's readme for detailed configuration and security notes.

## License

[MIT](LICENSE)

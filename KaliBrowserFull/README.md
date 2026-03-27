# KaliBrowserFull

Kali Linux with browser-based desktop access via noVNC. Access a full Kali desktop environment from any web browser on port 6080 — no VNC client needed.

## Build

```bash
docker build -t kali-browser KaliBrowserFull/
```

Note: This image installs `kali-linux-default`, which is a large download (~5GB+). The build will take some time.

## Run

```bash
docker run -d --name kali-browser -p 6080:6080 kali-browser
```

Open <http://localhost:6080> in your browser to access the Kali desktop.

## Configuration

| Setting            | Default  | Notes                                             |
| ------------------ | -------- | ------------------------------------------------- |
| Display resolution | 1024x768 | Change in `supervisord.conf` (Xvfb `-screen` arg) |
| Web port           | 6080     | noVNC web interface                               |
| VNC port           | 5900     | Internal only (not exposed by default)            |

To change the resolution, mount a custom supervisord config:

```bash
docker run -d --name kali-browser -p 6080:6080 \
    -v ./my-supervisord.conf:/etc/supervisor/conf.d/supervisord.conf \
    kali-browser
```

## Security Notes

- Runs as non-root user `kaliuser` (UID 1000)
- VNC has **no password** by default — do not expose port 6080 to untrusted networks
- For remote access, use an SSH tunnel: `ssh -L 6080:localhost:6080 your-server`
- This is a penetration testing toolkit — use responsibly and only on systems you have authorization to test

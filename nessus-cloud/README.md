# nessus-cloud

Nessus vulnerability scanner container that links to your [Tenable Cloud](https://cloud.tenable.com) account. Builds on Debian bookworm-slim with a parameterized Nessus version for easy updates.

## Prerequisites

Get your linking key from [cloud.tenable.com Scanners page](https://cloud.tenable.com/app.html#/scans/scanners) and save it to a file:

```bash
echo "YOUR_LINKING_KEY" > nessus.key
```

## Build

Uses [BuildKit secrets](https://docs.docker.com/build/building/secrets/) to avoid baking the key into image layers:

```bash
DOCKER_BUILDKIT=1 docker build \
    --secret id=nessus_key,src=./nessus.key \
    -t nessus-cloud nessus-cloud/
```

To build with a different Nessus version:

```bash
DOCKER_BUILDKIT=1 docker build \
    --secret id=nessus_key,src=./nessus.key \
    --build-arg NESSUS_VERSION=10.8.3 \
    -t nessus-cloud nessus-cloud/
```

## Run

```bash
docker run -d --name nessus-cloud -p 8834:8834 nessus-cloud
```

Access the web interface at <https://127.0.0.1:8834>

The scanner will appear as a linked scanner in your Tenable Cloud dashboard.

## Configuration

| Setting        | Default                   | Notes                                 |
| -------------- | ------------------------- | ------------------------------------- |
| Admin user     | `admin`                   | Change in `add_user.exp` before build |
| Admin password | `admin`                   | Change in `add_user.exp` before build |
| Web port       | 8834                      | Standard Nessus port                  |
| Scanner name   | `Nessus_Docker_Container` | Set via `NESSUS_NAME` env var         |

## Security Notes

- Runs as non-root user `nessususer` (UID 1000)
- Linking key is mounted as a BuildKit secret and never stored in image layers
- **Change the default admin credentials** in `add_user.exp` before building for any non-local use

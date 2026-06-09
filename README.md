# surdb — SurrealDB image (offline transport)

Holds the **SurrealDB server** Docker image as a loadable tarball, for importing
into an environment that can't pull from Docker Hub (e.g. UBS → Nexus).

| File | What it is |
|---|---|
| `surrealdb-v2-linux-amd64.tar.gz` | `surrealdb/surrealdb:v2`, **linux/amd64** (runs on AKS). `docker save` + gzip. |
| `surrealdb-v2-linux-amd64.tar.gz.sha256` | Integrity checksum for the tarball. |

- **Source image:** `surrealdb/surrealdb:v2` from https://hub.docker.com/r/surrealdb/surrealdb
- **Architecture:** `linux/amd64` (verified — NOT arm64)
- **amd64 digest:** `sha256:7db835aab6355b66e2b5779a3cddae811f201c417adadb4413310ae7f925110e`

## Import on the target side

```bash
# 1. verify it arrived intact
shasum -a 256 -c surrealdb-v2-linux-amd64.tar.gz.sha256

# 2. load into Docker  -> creates  surrealdb/surrealdb:v2
docker load < surrealdb-v2-linux-amd64.tar.gz

# 3. retag for Nexus and push
docker tag  surrealdb/surrealdb:v2  nexus.ubs.com:8443/docker-hosted/surrealdb/surrealdb:v2
docker push nexus.ubs.com:8443/docker-hosted/surrealdb/surrealdb:v2
```

That Nexus path is what the Kubernetes manifests reference. For production, pin
the image by the digest above rather than the moving `:v2` tag.

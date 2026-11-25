# Prometheus: ReadyStack build kit

The offline build kit for the Docker image `readystack/prometheus:v3.16.0-CE-ubuntu24.04-r4`. It holds what the image is built from (upstream source, base images, toolchain, pinned dependencies) and the documents, so the image can be rebuilt with no network access (`docker build --network=none`).

- Release: [v3.16.0-CE-ubuntu24.04-r4](https://github.com/readystack-dev/prometheus/releases/tag/v3.16.0-CE-ubuntu24.04-r4)
- Image: [`docker.io/readystack/prometheus:v3.16.0-CE-ubuntu24.04-r4`](https://hub.docker.com/r/readystack/prometheus/tags), manifest digest `sha256:9c5c22442864bc05e0e0c197e131e31c30b56da95a342501687f9f950770e3fe`
- Product page: https://readystack.dev/products/prometheus
- Defects: this repository's Issues (defects recorded from 25 November 2026 on) and https://readystack.dev/defects.html?product=prometheus

Prometheus is distributed under its upstream licence (Apache-2.0); the kit contains upstream's source under that licence, and each vendored component under its own.

## Download

From the [v3.16.0-CE-ubuntu24.04-r4 release](https://github.com/readystack-dev/prometheus/releases/tag/v3.16.0-CE-ubuntu24.04-r4): `ReadyStack-prometheus-v3.16.0-CE-ubuntu24.04-r4.tar.gz` (594.3 MB), SHA-256 `6e0a4bc93fb93c3ca3313f92f6114ae57c9c98163792e9fdcdb506dac253ee6a`.

```sh
sha256sum --check --ignore-missing SHA256SUMS
```

## Verify and rebuild

The rebuild statement is signed with the ReadyStack release key. Check the signature before you rely on it:

```sh
cosign verify-blob --key https://readystack.dev/keys/cosign.pub \
  --bundle ReadyStack-prometheus-v3.16.0-CE-ubuntu24.04-r4.rebuild.json.sigstore ReadyStack-prometheus-v3.16.0-CE-ubuntu24.04-r4.rebuild.json
```

Unpack with `-p` so file modes are kept, then follow the kit's own README.md and docs/REBUILD.md:

```sh
tar -xpzf ReadyStack-prometheus-v3.16.0-CE-ubuntu24.04-r4.tar.gz
cd v3.16.0-CE-ubuntu24.04-r4
less README.md docs/REBUILD.md
```

The build trail (`ReadyStack-prometheus-v3.16.0-CE-ubuntu24.04-r4.trail.json`, signed the same way, and its Markdown copy) records how this release was built and checked.

## This repository

`recipe/` holds the kit's recipe files (Dockerfiles, build scripts, pins) as published at https://readystack.dev/products/prometheus/recipe. The complete kit is the release download. Each release is a tag here.

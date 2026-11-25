# Prometheus: ReadyStack build kit

The offline build kit for the Docker image `readystack/prometheus:v3.16.0-CE-ubuntu24.04-r4`. It holds what the image is built from (upstream source, base images, toolchain, pinned dependencies) and the documents, so the image can be rebuilt with no network access (`docker build --network=none`).

- Release: [v3.16.0-CE-ubuntu24.04-r4](https://github.com/readystack-dev/prometheus/releases/tag/v3.16.0-CE-ubuntu24.04-r4)
- Image: [`docker.io/readystack/prometheus:v3.16.0-CE-ubuntu24.04-r4`](https://hub.docker.com/r/readystack/prometheus/tags), manifest digest `sha256:9c5c22442864bc05e0e0c197e131e31c30b56da95a342501687f9f950770e3fe`
- Product page: https://readystack.dev/products/prometheus
- Source and build recipe: https://readystack.dev/products/prometheus/recipe
- Build trail: https://readystack.dev/products/prometheus/trail/v3.16.0-CE-ubuntu24.04-r4
- Defects: this repository's Issues (defects recorded from 25 November 2026 on) and https://readystack.dev/defects.html?product=prometheus

## The image

Single container, single static Go binary, foreground process. Own embedded time-series database (TSDB) on disk; no relational database.

## Who uses Prometheus and why

Operations, SRE and platform teams who monitor servers and services and alert on problems.

Prometheus is a monitoring system and time series database. It collects metrics from configured targets at intervals, stores them with labels, evaluates rules written in PromQL, and triggers alerts.

A typical use: A team runs web services that expose request counts and latencies. Prometheus discovers the services, collects their metrics at intervals, and fires an alert when the error rate of any service stays above its threshold for several minutes.

From the upstream project's own description: https://github.com/prometheus/prometheus, https://prometheus.io/docs/introduction/overview/.

## This release

- Docker Scout policy grade at release: **A** (94%), graded 2026-11-22 with `docker scout policy` (CLI 1.26.0, docker-default configuration).
  - Docker Scout counts 11 packages with copyleft licences (GPL, LGPL, MPL), all of them operating-system packages (it counts only packages whose licence it can read; the SBOM lists every component).
- Signed with ReadyStack's release key (fingerprint `04daa92344a52dbc`): https://readystack.dev/keys/cosign.pub.
- Runs as a non-root user (`nobody`).
- Attestations on the image: an SPDX SBOM and SLSA provenance v1.
- Rebuilt from this kit with network egress blocked on 2026-11-22; the rebuilt image digest is the published one.
- Base: Ubuntu 24.04.

The current grade, measured again every day, is on https://readystack.dev/products/prometheus.

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

— ReadyStack · https://readystack.dev

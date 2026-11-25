# Prometheus v3.16.0 — ReadyStack release `v3.16.0-CE-ubuntu24.04-r4`

Generated 2026-11-22 from this release's build records and strategy. Statements that come from a single record name it in brackets; the others come from this release's strategy and build log.

## Release

- **Image:** `readystack/prometheus:v3.16.0-CE-ubuntu24.04-r4` (linux/amd64). Index digest `sha256:11973f133696e92b6dba03aaac57cfdf5c076c80ab8ccf94b0d2d9df5a204589` [`docs/provenance/LOCKED_DIGEST.txt`].
- **Image manifest digest:** `sha256:9c5c22442864bc05e0e0c197e131e31c30b56da95a342501687f9f950770e3fe` [`TARGET_DIGEST.txt`].
- **Upstream:** Prometheus v3.16.0 built from source at `v3.16.0` of https://github.com/prometheus/prometheus — upstream notes: https://github.com/prometheus/prometheus/releases/tag/v3.16.0 [`docs/provenance/BUILD_RECIPE.json`].
- **Base image:** none (`FROM scratch`): a chiselled Ubuntu 24.04 runtime, the files Canonical's chisel cut from Ubuntu's own packages (`vendored/chisel/slices.txt`, `vendored/chisel/packages.tsv`); runtime user `nobody`.
- **Build:** `source-rebuild-go`, compiled offline with `--network=none`.
- **Supersedes:** ReadyStack release `v3.16.0-CE-ubuntu24.04-r3`.

## What changed in this release

- Upstream version unchanged (v3.16.0); ReadyStack rebuild.
- Minimum dependency versions ReadyStack enforces on top of upstream (raised only where upstream's own version was lower); shipped versions from the SBOM [`docs/sbom/SBOM.json`]:
  - `golang.org/x/crypto` at least `v0.56.0`: not raised, upstream already pins `v0.58.0` (shipped: `v0.58.0`)
  - `google.golang.org/grpc` at least `v1.83.2`: not raised, upstream already pins `v1.84.0` (shipped: `v1.84.0`)
  - `golang.org/x/net` raised from upstream's `v0.61.0` to at least `v0.62.0` (shipped: `v0.62.0`)
  - `github.com/klauspost/compress` at least `v1.18.7`: not raised, upstream already pins `v1.20.3` (shipped: `v1.20.3`)
- Prometheus v3.16.0 is unchanged from the release this supersedes (v3.16.0-CE-ubuntu24.04-r3); this release rebuilds it. Changes: the image is rebuilt on the current ubuntu:24.04 image (sha256:9c7f200c07e3…), which its maintainers updated after v3.16.0-CE-ubuntu24.04-r3 was built on sha256:2198e4c65076…. Settings, ports and data volumes are those of v3.16.0-CE-ubuntu24.04-r3: replace the container with this image on the same volumes.

## Security status at release (measured 2026-11-20)

- **Docker Scout:** 0 fixable critical/high vulnerabilities; runs as `nobody` (non-root); SBOM + provenance attestations present (SPDX SBOM + SLSA v1) [`docs/provenance/SCOUT_VERDICT.json`].
- **OSV cross-check:** no fixable critical/high vulnerability beyond Docker Scout's findings.
- **Signature:** cosign signature verified by ReadyStack's release gate before publishing; check it yourself with `cosign verify --key https://readystack.dev/keys/cosign.pub readystack/prometheus:v3.16.0-CE-ubuntu24.04-r4` (signatures are recorded in the public Sigstore transparency log).
- **ReadyStack release check at release:** passed (ReadyStack's own test, not a Docker Scout grade). Vulnerabilities are disclosed every day and the image never changes, so it is re-checked daily, beside Docker Scout's own dated grade, at https://readystack.dev/queue.
- **Before exposing this service:** Prometheus has no login by default: anyone who reaches port 9090 can read every metric. Keep it on a private network, or add basic authentication (bcrypt hashes) and TLS with --web.config.file, overriding the health check with --health-cmd because basic authentication also covers /-/healthy. It covers /metrics too, so Prometheus's own scrape of itself is refused (target down, up = 0) until that job is given the password; the steps are under Password protection (basic authentication) in the handbooks.

## Reproducibility

Rebuilds offline from this archive to the identical image digest `sha256:9c5c22442864bc05e0e0c197e131e31c30b56da95a342501687f9f950770e3fe`: run `./BUILD_OFFLINE.sh`, then `./VERIFY_OFFLINE.sh` (add `--online` to confirm the Docker Hub tag holds the same digest). Before publishing, ReadyStack rebuilt the kit with `./BUILD_OFFLINE.sh` and compared the result with the published digest [`docs/provenance/REPRO_VERIFICATION.json`], then ran `./BUILD_OFFLINE.sh` and `./VERIFY_OFFLINE.sh` from this archive (without `--online`).

## Validation

- 4 of 4 use cases passed (4 authored) against the running image [`docs/validation/test-results.json`].

## Known issues

- **high** (upstream): Unauthenticated access by default [`RSD-prometheus-b3e31c3dde`]

## Defects fixed

- 11 problems found before release and fixed before it shipped (11 by review); each is listed in `docs/defects/defects.json`.

## Verify this archive

```
sha256sum -c CHECKSUMS.sha256
./BUILD_OFFLINE.sh && ./VERIFY_OFFLINE.sh --online
cosign verify --key https://readystack.dev/keys/cosign.pub readystack/prometheus:v3.16.0-CE-ubuntu24.04-r4
```

ReadyStack records every signature in the public Sigstore transparency log (Rekor), so the command needs no extra flags.


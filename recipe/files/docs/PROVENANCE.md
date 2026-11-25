# Provenance — Prometheus (`v3.16.0-CE-ubuntu24.04-r4`)

Every item below is copied from a record in this archive; the record is named in brackets.

## Source

- Upstream repository: https://github.com/prometheus/prometheus
- Revision built: `v3.16.0` (commit `e2e28adeee2c5883d000fc82aca6008cfbf7e96d`)
- Source tree: `dbee8a20cb6bdde803a797b6ba7906ee1c8a010292fce25e73721ed51e1c0980` over 1714 files [`docs/provenance/SOURCE_TREE_HASH.json`]
- Community Edition only: enterprise code is removed or refused before the build (CE audit at acquire).

## Build

- Method: `source-rebuild-go`; base image `scratch + a chiselled Ubuntu 24.04 runtime (vendored/chisel/slices.txt, packages.tsv)`
- Compiled with `--network=none` from vendored source, dependencies and base images; BuildKit pinned to `moby/buildkit:v0.31.2` (shipped in `toolchain/`).
- SOURCE_DATE_EPOCH: `1795161192` [`SOURCE_DATE_EPOCH.txt`]
- Frozen inputs: [`docs/provenance/FREEZE_MANIFEST.json`], build recipe: [`docs/provenance/BUILD_RECIPE.json`].

## Output

- Published image: `readystack/prometheus:v3.16.0-CE-ubuntu24.04-r4` — index digest `sha256:11973f133696e92b6dba03aaac57cfdf5c076c80ab8ccf94b0d2d9df5a204589` [`docs/provenance/LOCKED_DIGEST.txt`]
- linux/amd64 image digest `sha256:9c5c22442864bc05e0e0c197e131e31c30b56da95a342501687f9f950770e3fe` — reproduced from this archive before publishing [`docs/provenance/REPRO_VERIFICATION.json`].

## Attestations and signature

- SBOM + SLSA provenance attached to the image: attached: an SPDX SBOM and SLSA v1 provenance [`docs/provenance/SCOUT_VERDICT.json`]
- cosign signature by the ReadyStack release key: verified before publishing. Check it: `cosign verify --key https://readystack.dev/keys/cosign.pub readystack/prometheus:v3.16.0-CE-ubuntu24.04-r4`

## Pinned inputs, and why

- 31 pinned inputs, each with where it came from, why this one, how it was checked and when it was decided: `docs/PINS.md` [`docs/provenance/PINS.json`] (upstream source 2, base and build images 3, image digests 2, language runtimes and toolchains 1, dependency versions readystack sets 6, pins in the kit's dockerfiles 10, readystack's build engine 5, dependencies upstream chose 2).

## Build trail

- How this release passed through the ReadyStack harness, record by record (every chain, drive, stage, review round, defect and check, each naming the record it is copied from): `ReadyStack-prometheus-v3.16.0-CE-ubuntu24.04-r4.trail.json`, published beside this archive and signed with the release key (`ReadyStack-prometheus-v3.16.0-CE-ubuntu24.04-r4.trail.json.sigstore`), and readable at https://readystack.dev/products/prometheus/trail/v3.16.0-CE-ubuntu24.04-r4.


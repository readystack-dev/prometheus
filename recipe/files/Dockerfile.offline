# GENERATED L-009 hermetic build (--network=none, SBOM+SLSA-attestation-producing).
# Pinned bases from BASE_IMAGE_DIGESTS.lock; vendored Go tree + debs from Dockerfile.vendor.
FROM golang:1.26.11-bookworm@sha256:dc40a13b61304647ed7e5d581d246b86413dfc29d63c6d018310feb93cd98fc0 AS build_prometheus
WORKDIR /src
COPY vendored/prometheus-src/ .
ENV CGO_ENABLED=0 GOTOOLCHAIN=local GOWORK=off GOFLAGS="-mod=vendor -buildvcs=false" GOPROXY=off SOURCE_DATE_EPOCH=1795161192
RUN go build -trimpath -tags "builtinassets" -ldflags='-s -w -X github.com/prometheus/common/version.Version=3.16.0 -X github.com/prometheus/common/version.Branch=HEAD' -o /prometheus ./cmd/prometheus && /prometheus --version && \
    { readelf -lW /prometheus | sed -n 's/.*Requesting program interpreter: \(.*\)]$/\1/p'; readelf -dW /prometheus | sed -n 's/.*Shared library: \[\(.*\)\]$/\1/p'; } > /prometheus.needs
FROM golang:1.26.11-bookworm@sha256:dc40a13b61304647ed7e5d581d246b86413dfc29d63c6d018310feb93cd98fc0 AS build_promtool
WORKDIR /src
COPY vendored/prometheus-src/ .
ENV CGO_ENABLED=0 GOTOOLCHAIN=local GOWORK=off GOFLAGS="-mod=vendor -buildvcs=false" GOPROXY=off SOURCE_DATE_EPOCH=1795161192
RUN go build -trimpath -tags "builtinassets" -ldflags='-s -w -X github.com/prometheus/common/version.Version=3.16.0' -o /promtool ./cmd/promtool && /promtool --version && \
    { readelf -lW /promtool | sed -n 's/.*Requesting program interpreter: \(.*\)]$/\1/p'; readelf -dW /promtool | sed -n 's/.*Shared library: \[\(.*\)\]$/\1/p'; } > /promtool.needs
# rs-probe: the HEALTHCHECK helper (standard library only, from the kit's rs-probe.go)
FROM golang:1.26.11-bookworm@sha256:dc40a13b61304647ed7e5d581d246b86413dfc29d63c6d018310feb93cd98fc0 AS build_rs_probe
WORKDIR /probe
COPY rs-probe.go main.go
COPY rs-probe.mod go.mod
ENV CGO_ENABLED=0 GOTOOLCHAIN=local GOFLAGS="-buildvcs=false" GOPROXY=off SOURCE_DATE_EPOCH=1795161192
RUN go build -trimpath -ldflags='-s -w' -o /rs-probe .
# the chiselled rootfs (vendored/chisel, cut by Dockerfile.vendor), the service user, the binaries
FROM ubuntu:24.04@sha256:9c7f200c07e3d4a03e7249d2c16bf66a51120fb876aa2242cf31451412543289 AS rootfs
ARG SOURCE_DATE_EPOCH=1795161192
ADD vendored/chisel/rootfs.tar /rootfs/
RUN grep -q '^nobody:[^:]*:65534:65534:' /rootfs/etc/passwd && grep -q '^nogroup:[^:]*:65534:' /rootfs/etc/group && \
    mkdir -p /rootfs/prometheus /rootfs/etc/prometheus && chown -R 65534:65534 /rootfs/prometheus /rootfs/etc/prometheus
COPY --from=build_prometheus /prometheus /rootfs/usr/bin/prometheus
COPY --from=build_prometheus /prometheus.needs /tmp/needs/prometheus
COPY --from=build_promtool /promtool /rootfs/usr/bin/promtool
COPY --from=build_promtool /promtool.needs /tmp/needs/promtool
COPY --from=build_rs_probe /rs-probe /rootfs/usr/local/bin/rs-probe
COPY prometheus.yml /rootfs/etc/prometheus/prometheus.yml
RUN chmod 755 /rootfs/usr/bin/prometheus /rootfs/usr/bin/promtool /rootfs/usr/local/bin/rs-probe && \
    chown 65534:65534 /rootfs/etc/prometheus/prometheus.yml
RUN --mount=type=bind,source=vendored/chisel,target=/chisel set -eu; \
    for n in /tmp/needs/*; do while read -r lib; do [ -n "$lib" ] || continue; \
      case "$lib" in /*) [ -e "/rootfs$lib" ] ;; *) [ -n "$(find /rootfs/usr/lib /rootfs/usr/lib64 -name "$lib" 2>/dev/null | head -1)" ] ;; esac || \
      { echo "$(basename "$n") needs $lib, which the chiselled rootfs does not have: add its slice to runtime_base.slices" >&2; exit 1; }; \
    done < "$n"; done; rm -rf /tmp/needs; \
    [ -e /rootfs/etc/os-release ] || { echo "no /etc/os-release: base-files_release-info is missing" >&2; exit 1; }; \
    [ "$(grep -c '^Package: ' /rootfs/var/lib/dpkg/status)" = "$(wc -l < /chisel/packages.tsv)" ] || \
      { echo "/var/lib/dpkg/status does not list every package of vendored/chisel/packages.tsv" >&2; exit 1; }; \
    rm -rf /rootfs/var/log/dpkg.log /rootfs/var/log/alternatives.log /rootfs/var/log/apt /rootfs/var/cache/ldconfig/aux-cache /rootfs/var/cache/fontconfig
FROM scratch
ARG SOURCE_DATE_EPOCH=1795161192
COPY --from=rootfs /rootfs/ /
ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
LABEL org.opencontainers.image.licenses="Apache-2.0"
LABEL org.opencontainers.image.title="Prometheus"
LABEL org.opencontainers.image.description="Prometheus collects and stores time-series data for monitoring infrastructure and applications. It provides a robust query language and alerting capabilities to help teams track system performance and detect anomalies effectively."
LABEL org.opencontainers.image.source="https://github.com/prometheus/prometheus"
LABEL org.opencontainers.image.version="v3.16.0-CE-ubuntu24.04-r4"
LABEL org.opencontainers.image.revision="e2e28adeee2c5883d000fc82aca6008cfbf7e96d"
HEALTHCHECK --interval=5s --timeout=30s --start-period=10s \
  CMD /usr/local/bin/rs-probe http://127.0.0.1:9090/-/healthy || exit 1
USER nobody
EXPOSE 9090
ENTRYPOINT ["prometheus", "--config.file=/etc/prometheus/prometheus.yml", "--storage.tsdb.path=/prometheus", "--web.listen-address=:9090"]

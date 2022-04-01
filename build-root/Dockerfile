ARG bazelisk_version=v1.11.0
ARG bazel_version=5.1.0

FROM --platform=linux/amd64 curlimages/curl:7.82.0 as builder

ARG bazelisk_version
ARG bazel_version

USER root

WORKDIR /build

RUN curl -L https://github.com/bazelbuild/bazelisk/releases/download/${bazelisk_version}/bazelisk-linux-amd64 -o bazelisk && \
    chmod a+x bazelisk

ENV USE_BAZEL_VERSION ${bazel_version}
ENV XDG_CACHE_HOME /build/cache
RUN /build/bazelisk || true
RUN mv /build/cache/bazelisk/downloads/bazelbuild/bazel-${bazel_version}-linux-x86_64/bin/bazel /build/bazel

FROM gcr.io/distroless/java17-debian11

WORKDIR /app

ENV PATH="/app/bin:${PATH}"

COPY --from=builder /build/bazelisk /app/bin/bazelisk
COPY --from=builder /build/bazel /app/bin/bazel

ENTRYPOINT ["/app/bin/bazel"]
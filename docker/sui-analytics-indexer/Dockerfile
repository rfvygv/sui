FROM rust:1.85-bullseye as build

ARG PROFILE=release
WORKDIR /work

RUN apt-get update && apt-get install -y cmake clang

COPY .git/ .git/
COPY Cargo.toml Cargo.lock ./
COPY consensus consensus
COPY crates crates
COPY sui-execution sui-execution
COPY external-crates external-crates

RUN cargo build --profile ${PROFILE} --bin sui-analytics-indexer

FROM debian:12-slim as deploy
RUN apt-get update && apt-get install -y --no-install-recommends ca-certificates
COPY --from=build --chmod=755 /work/target/release/sui-analytics-indexer /opt/sui/bin/sui-analytics-indexer

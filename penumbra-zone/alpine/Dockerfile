FROM alpine:3.18.3

EXPOSE 3000

MAINTAINER Kirigum
LABEL version="0.0.1"

# install packages
RUN apk update && \
  apk add --no-cache git-lfs make curl go build-base pkgconfig libressl-dev clang musl-dev libgcc

# init rust
RUN mkdir -p /user/turreta-rust-builder/src && \
  cd /user/turreta-rust-builder/src && \
  curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
ENV PATH="/root/.cargo/bin:${PATH}"

# clone penumbra & tendermint
RUN git clone https://github.com/penumbra-zone/penumbra.git && \
  cd penumbra/ && \
  git fetch && \
  cd ../ && \
  git clone https://github.com/tendermint/tendermint.git && \
  cd tendermint/ && \
  git fetch && \
  git checkout v0.34.23

# install tendermint
RUN cd tendermint/ && \
  make install && \
  make build

# RUN apk add --no-cache -U build-base pkgconfig libressl-dev clang musl-dev libgcc

# build pcli & pd
RUN cd penumbra/ && \
  RUSTFLAGS="-C target-feature=-crt-static" cargo build --release --bin pcli && \
  RUSTFLAGS="-C target-feature=-crt-static" cargo build --release --bin pd


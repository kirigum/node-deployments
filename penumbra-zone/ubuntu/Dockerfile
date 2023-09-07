FROM ubuntu:22.04

EXPOSE 3000

MAINTAINER Kirigum
LABEL version="0.0.1"

# install packages
RUN apt-get update && \
  apt-get upgrade && \
  apt-get install -y --no-install-recommends build-essential pkg-config libssl-dev clang git-lfs make curl golang-go ca-certificates && \
  rm -rf /var/lib/apt

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

# build pcli & pd
RUN cd penumbra/ && \
  cargo build --release --bin pcli && \
  cargo build --release --bin pd

# install tendermint
RUN cd tendermint/ && \
  make install && \
  make build


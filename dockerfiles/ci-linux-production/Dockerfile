# mainly copied from parity/scripts repo
FROM debian:bullseye-slim

RUN set -eux; \
  apt-get -y update; \
  apt-get install -y --no-install-recommends \
  libssl-dev clang lld libclang-dev make cmake \
  git pkg-config curl time rhash ca-certificates lsof; \
  update-alternatives --install /usr/bin/cc cc /usr/bin/clang 100

ENV RUSTUP_HOME=/usr/local/rustup \
  CARGO_HOME=/usr/local/cargo \
  PATH=/usr/local/cargo/bin:$PATH \
  CC=clang \
  CXX=clang

# install rustup, use minimum components
RUN  curl -L "https://static.rust-lang.org/rustup/dist/x86_64-unknown-linux-gnu/rustup-init" \
  -o rustup-init; \
  chmod +x rustup-init; \
  ./rustup-init -y --no-modify-path --profile minimal --default-toolchain nightly-2022-01-19; \
  rm rustup-init; \
  chmod -R a+w ${RUSTUP_HOME} ${CARGO_HOME}; \
  rustup show; \
  cargo --version \
  rm -rf "${CARGO_HOME}/registry" "${CARGO_HOME}/git" /root/.cache/sccache; \
  # apt clean up
  apt-get autoremove -y; \
  apt-get clean; \
  # add non-root user
  groupadd -g 1000 nonroot; \
  useradd -u 1000 -g 1000 -s /bin/bash -m nonroot

ENV	RUST_BACKTRACE=1

RUN set -eux && \
  # install `rust-src` component for ui test
  rustup component add rust-src && \
  # install wasm toolchain
  rustup target add wasm32-unknown-unknown && \
  # versions
  rustup show && \
  cargo --version

# apt clean up
RUN  apt-get autoremove -y && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*

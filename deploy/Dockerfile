FROM rust:1.46.0-slim-buster AS build

RUN apt-get update && apt-get install -y sudo

RUN cargo install --version "0.1.3" cargo-build-dependencies
RUN mkdir /src && cd /src && USER=root cargo new --bin testcase2

COPY Cargo.toml Cargo.lock ./src/testcase2/
RUN echo "This should be cached"

COPY . /src/testcase2
WORKDIR /src/testcase2

RUN echo "cargo build would go here"

FROM busybox:1.32.0-glibc
COPY --from=build /src/testcase2/Cargo.toml /
COPY --from=build /lib/x86_64-linux-gnu/libdl.so.2 /lib/
COPY --from=build /lib/x86_64-linux-gnu/librt.so.1 /lib/
COPY --from=build /lib/x86_64-linux-gnu/libgcc_s.so.1 /lib/
ENTRYPOINT ["/bin/sh"]

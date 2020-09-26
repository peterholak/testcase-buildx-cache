# This dockerfile attemps to reproduce the issue in a quick and minimal-ish fashion.
# See Dockerfile.real for a more realistic case.

FROM busybox:1.32.0 AS build

RUN mkdir -p /src/testcase2 && cd /src/testcase2

# These 2 files specify the dependencies, which have to be rebuilt only if these files change.
COPY Cargo.toml Cargo.lock ./src/testcase2/
WORKDIR /src/testcase2
# Replacing actual dependecy building with a dummy command.
RUN echo "Here the dependencies would be built. This should be cached."

# The changed source file only gets copied in this step.
COPY . /src/testcase2/

# This would be the main build command of the app.
RUN echo "cargo build would go here"

FROM busybox:1.32.0-glibc
# Here the build artifacts would be copied into the final image.
COPY --from=build /src/testcase2/Cargo.toml /
# This would normally run the build artifact from previous stage.
ENTRYPOINT ["/bin/sh"]

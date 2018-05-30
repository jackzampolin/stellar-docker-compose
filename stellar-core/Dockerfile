FROM debian:stretch AS build

# Build Arguements
ARG STELLAR_CORE_VERSION="v9.2.0"
ARG STELLAR_CORE_BUILD_DEPS="git build-essential pkg-config autoconf automake libtool bison flex libpq-dev wget pandoc"

# Install build deps
RUN apt-get update && apt-get install -y $STELLAR_CORE_BUILD_DEPS

# Clone the stellar-core repo
RUN git clone --branch $STELLAR_CORE_VERSION --recursive --depth 1 https://github.com/stellar/stellar-core.git /src/stellar-core

WORKDIR /src/stellar-core

# Install stellar-core
RUN ./autogen.sh && \
    ./configure && \
    make && \
    make install

# Finish build
FROM debian:stable-slim

# Build Arguements
ARG STELLAR_CORE_DEPS="curl libpq5"

# Install runtime deps
RUN apt-get update && \
    apt-get install -y $STELLAR_CORE_DEPS

COPY --from=build /usr/local/bin/stellar-core /usr/local/bin/stellar-core

# Peer Network -> 11625
# HTTP Server  -> 11626 (NOTE: Do not expose publically)
EXPOSE 11625-11626

COPY ./stellar-core-testnet.cfg /stellar-core.cfg

COPY ./entry.sh /
ENTRYPOINT ["/entry.sh"]

CMD ["stellar-core", "--conf", "/stellar-core.cfg"]

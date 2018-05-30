FROM golang:alpine AS build

# git tag for horizon release from https://github.com/stellar/go/releases
ARG HORIZON_VERSION="v0.12.3"
ARG GO_MONOREPO="github.com/stellar/go"

# Install apk deps and glide
RUN apk update && \
    apk add git curl mercurial sqlite build-base bash && \
    curl https://glide.sh/get | sh

# Clone source files
RUN git clone --branch horizon-$HORIZON_VERSION https://$GO_MONOREPO /go/src/$GO_MONOREPO

WORKDIR /go/src/$GO_MONOREPO

# Install golang dependancies
RUN glide i

WORKDIR /go/src/$GO_MONOREPO/services/horizon

# Install Horizon
RUN go install ./...

FROM alpine

RUN apk add --update \
  ca-certificates \
  bash

EXPOSE 8000

COPY --from=build /go/bin/horizon /usr/bin/horizon

COPY entry.sh /
ENTRYPOINT ["/entry.sh"]

CMD ["horizon"]

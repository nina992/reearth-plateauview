FROM golang:1.18-alpine AS build
ARG TAG=release
ARG VERSION

RUN apk add --update --no-cache git ca-certificates build-base

COPY go.mod go.sum main.go /app/
WORKDIR /app
RUN go mod download

COPY pkg/ /app/pkg/
COPY internal/ /app/internal/

RUN CGO_ENABLED=0 go build -tags "${TAG}" "-ldflags=-X main.version=${VERSION} -s -w -buildid=" -trimpath

FROM scratch

COPY --from=build /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt
COPY --from=build /app/server /app/reearth-plateauview

WORKDIR /app

CMD [ "./reearth-plateauview" ]

# Vendor stage
FROM golang:1.16 as dep
WORKDIR /build
COPY go.mod go.sum ./
RUN GOPRIVATE=github.com/pixiespirit GO111MODULE=on go mod download
COPY . .
RUN go mod vendor

# Build binary stage
FROM golang:1.16 as build
WORKDIR /build
COPY --from=dep /build .
RUN CGO_ENABLED=0 GOOS=linux go build -mod=vendor -a -installsuffix cgo -o server 

# Minimal image
FROM alpine:latest
WORKDIR /app
COPY --from=build /build/server server
RUN apk update
RUN apk upgrade
CMD ["./server"]
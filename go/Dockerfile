# sample Dockerfile for remotesrv
FROM golang:1.24 AS builder

# Add required packages for CGo zstd (Debian-based)
RUN apt-get update && apt-get install -y \
  build-essential \
  libzstd-dev \
  && rm -rf /var/lib/apt/lists/*

WORKDIR /app
COPY . .
RUN go mod tidy

# Build the remotesrv binary
WORKDIR /app/utils/remotesrv
RUN GOOS=linux GOARCH=arm64 go build -o remotesrv main.go

# Minimal runtime image
FROM debian:bullseye-slim
COPY --from=builder /app/utils/remotesrv/remotesrv /usr/local/bin/remotesrv
EXPOSE 80 50051
ENTRYPOINT ["/usr/local/bin/remotesrv"]
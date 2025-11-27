# Build stage
FROM golang:1.25.4-alpine3.21 AS build

WORKDIR /app

# Copy all necessary files
COPY . .
COPY go.mod .
COPY go.sum .

# Set Go proxy
ENV GOPROXY=https://proxy.golang.org,direct
ENV DUBBO_GO_CONFIG_PATH=/conf/dubbogo.yaml

# Download dependencies and build the Go app
RUN go mod download
RUN go build -o server cmd/server.go

# Final stage: minimal image with the compiled binary
FROM alpine:3.22.2

# Install necessary certificates for HTTPS connections
RUN apk --no-cache add ca-certificates

WORKDIR /root/

# Copy the compiled Go binary and the 'conf' directory from the build stage
COPY --from=build /app/server .
COPY --from=build /app/conf /conf

# Set the entrypoint to the server binary
CMD ["./server"]

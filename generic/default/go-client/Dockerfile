FROM golang:1.25.4-alpine3.21 AS build

WORKDIR /app
COPY . .
COPY go.mod .
COPY go.sum .
RUN go mod tidy

ENV GOPROXY=https://proxy.golang.org,direct
ENV GOPROXY=https://proxy.golang.org,direct
ENV DUBBO_GO_CONFIG_PATH=./conf/dubbogo.yaml

RUN go mod download
RUN go build -o client cmd/client.go

FROM alpine:3.22.2
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=build /app/client .

CMD ["./client"]

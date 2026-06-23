FROM golang:1.21-alpine AS builder
RUN apk add --no-cache git ca-certificates
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN go mod tidy
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -ldflags="-s -w" -o /app/evaluation-service .
FROM alpine:3.19
RUN apk add --no-cache ca-certificates tzdata
RUN addgroup -S app && adduser -S app -G app
USER app
WORKDIR /app
COPY --from=builder /app/evaluation-service .
EXPOSE 8004
ENTRYPOINT ["./evaluation-service"]
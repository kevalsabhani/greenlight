# ========== BUILD STAGE ==========
FROM golang:1.23.1-alpine AS builder

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .

RUN apk add --no-cache make git

RUN make build/api

# ========== RUN STAGE ==========
FROM alpine:latest

RUN apk add --no-cache ca-certificates

WORKDIR /root/

COPY --from=builder /app/bin/linux_amd64 .

COPY --from=builder /app/docker/entrypoint.sh .

EXPOSE 8080

RUN chmod +x ./entrypoint.sh

ENTRYPOINT [ "./entrypoint.sh" ]
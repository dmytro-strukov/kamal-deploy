FROM golang:1.24.1 AS builder
WORKDIR /app
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo app.go

# The run stage
FROM scratch
WORKDIR /app
COPY --from=builder /app/app .
EXPOSE 80
CMD ["./app"]

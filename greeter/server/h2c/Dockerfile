FROM golang:1.12.5 as builder

LABEL maintainer="jearzamendia@gmail.com"

# Set the Current Working Directory inside the container
WORKDIR /go/src/github.com/jarzamendia/grpc-server-h2c

COPY get.sh .

RUN bash get.sh

# Copy everything from the current directory to the PWD(Present Working Directory) inside the container
COPY . .

# Download dependencies
RUN go get -d -v ./...

# Build the Go app
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o /go/bin/grpc-server-h2c .

######## Start a new stage from scratch #######
FROM alpine:3.9  

WORKDIR /root

# Copy the Pre-built binary file from the previous stage
COPY --from=builder /go/bin/grpc-server-h2c .

EXPOSE 50051

ENTRYPOINT ["./grpc-server-h2c"]
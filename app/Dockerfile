FROM golang:1.22

RUN apt-get update && apt-get install -y protobuf-compiler && \
    go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.26 && \
    go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.1

WORKDIR /usr/src/app

COPY go.mod go.sum ./
RUN go mod download && go mod verify

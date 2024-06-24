FROM golang:1.18 as builder
WORKDIR /workspace
COPY main.go .
RUN go mod init foo && go mod tidy
RUN go build -o /workspace/main

# docker multi-stages build
FROM ubuntu:20.04
# ENV GIN_MODE=release
WORKDIR /
COPY --from=builder /workspace/main .
RUN chmod +x ./main
EXPOSE 8080
CMD [ "./main" ]
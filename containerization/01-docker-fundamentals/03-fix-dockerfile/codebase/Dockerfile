FROM golang:1.22
WORKDIR /app
COPY . .
RUN go build -o server .
EXPOSE 8080
ENV GIN_MODE=release
USER root
CMD ["./server"]

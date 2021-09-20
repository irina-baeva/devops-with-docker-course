FROM golang:1.16

WORKDIR /app

COPY . /app/

EXPOSE 8080 

#Run go build. It will generate a binary named "server"
RUN go build

#Run tests with
RUN go test ./...

CMD ["./server"]
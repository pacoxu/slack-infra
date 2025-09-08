FROM golang:1.23-alpine AS build-env

# All these steps will be cached
RUN mkdir /slack-inactive-detector
WORKDIR /slack-inactive-detector

# Copy the module files first and then download the dependencies. If this
# doesn't change, we won't need to do this again in future builds.
COPY go.* /slack-inactive-detector/
RUN go mod download

# Copy the source code and build the app.
COPY . /slack-inactive-detector
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -a -installsuffix cgo -o slack-inactive-detector ./slack-inactive-detector

# Runtime container
FROM alpine:latest  
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=build-env /slack-inactive-detector/slack-inactive-detector .
CMD ["./slack-inactive-detector"]
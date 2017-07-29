FROM golang:1.8.3-alpine AS build

ADD . /go/src/github.com/restic/rest-server
WORKDIR /go/src/github.com/restic/rest-server

RUN go run build.go

# FINAL STAGE

FROM alpine:3.6

RUN apk add --no-cache --update apache2-utils

COPY docker/entrypoint.sh docker/create_user docker/delete_user /usr/local/bin/
COPY --from=build /go/src/github.com/restic/rest-server/rest-server /usr/local/bin/

ENV DATA_DIRECTORY=/data
ENV PASSWORD_FILE=/data/.htpasswd

VOLUME /data
EXPOSE  80

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
CMD rest-server --listen ":80" --path $DATA_DIRECTORY

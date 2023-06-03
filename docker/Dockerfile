FROM alpine:edge

COPY entrypoint /usr/local/bin/entrypoint

RUN apk add --no-cache docker-cli pigz

ENTRYPOINT ["/usr/local/bin/entrypoint"]

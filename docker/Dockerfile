FROM alpine:latest AS builder

RUN apk add --no-cache \
    gcc \
    make \
    musl-dev \
    libc-dev

WORKDIR /build
COPY ratelimit.c redismodule.h Makefile ./
RUN make

FROM redis:8-alpine

COPY --from=builder /build/ratelimit.so /usr/local/lib/ratelimit.so
COPY docker/redis.conf /usr/local/etc/redis/redis.conf

CMD [ "redis-server", "/usr/local/etc/redis/redis.conf" ]

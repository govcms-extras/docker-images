FROM alpine:latest

RUN apk --no-cache add dnsmasq-dnssec

EXPOSE 53 53/udp

ENTRYPOINT ["dnsmasq", "-k"]
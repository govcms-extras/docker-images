# docker-images

## Release images

### Release dnsmasq image

```bash
docker buildx build --platform linux/amd64,linux/arm64 --tag govcmsextras/dnsmasq --push .
```
# nemousu.live-on.net-redirector-image

Container image (Caddy + config) that redirects
`https://nemousu.live-on.net/bbs/search?identifier=...`
to `https://nemousu.pages.dev/?id=...`.

## Build & Run

```bash
docker build -t nemousu-redirector .
docker run --rm -p 80:80 -p 443:443 nemousu-redirector
```

This container is intended to run publicly for the host
`nemousu.live-on.net` (configurable via environment variable
`SITE_HOST`). Make sure the domain’s DNS `A/AAAA` record points at the
server that runs the container, then expose ports 80/443 through your
firewall/load balancer so Caddy can solve the HTTP-01 challenge and
obtain certificates from Let’s Encrypt automatically. Both HTTP and
HTTPS endpoints respond with a permanent redirect to
`https://nemousu.pages.dev/?id=...`; HTTP does **not** redirect to HTTPS
first, ensuring clients immediately land on the Pages URL.

For local smoke tests, hit
`curl -I "http://127.0.0.1/bbs/search?identifier=fDWFMWbqMU"`. HTTPS
tests require the real hostname and a public certificate, so they only
succeed once the container is deployed behind `nemousu.live-on.net`.

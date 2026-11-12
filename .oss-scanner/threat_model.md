# Threat model

## What this project does and where untrusted input enters
A web server and reverse proxy (Go, Caddy). Untrusted input: HTTP(S) requests (all of them),
the Caddyfile/JSON configuration, and TLS handshakes. Request parsing (HTTP/1.1, HTTP/2,
websockets) and config loading are the main surfaces.

## Components that matter most / least
- most: the HTTP request parsing and the reverse-proxy path, plus config parsing.
- less: the CLI, docs.

## How to exercise it
Send crafted HTTP/1.1 and HTTP/2 requests; load crafted Caddyfile/JSON config; test the proxy
path with hostile upstream responses.

## How you rate severity
- Memory-safety or RCE bugs in request/config parsing: critical. SSRF or request-smuggling
  logic bugs: high. DoS: medium.

## Anything to leave alone
- Do not report issues only reachable via a trusted local admin as high severity.

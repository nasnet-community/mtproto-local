# mtproto-local

Local MTProto: a plugin for the NASNET panel on MikroTik routers that runs a Telegram
MTProto proxy ([telemt](https://github.com/telemt/telemt)) on the router itself. Phones
reach it through the router's public address, and its page in the panel manages the
users and their links.

The image is `ghcr.io/nasnet-community/mtproto-local`, for amd64, arm64 and 32-bit ARM.

## What is here

| Path | What it is |
|---|---|
| `cmd/mtproto-local` | the plugin: serves the page and its API, and runs telemt beside it in the container |
| `internal/local` | its store, the proxy's supervision, the router's side (port, public address) and the API |
| `internal/plugin/users`, `userapi`, `ui` | users, their API and the page, shared with Remote MTProto |
| `web` | the page's source; `npm run build:local` builds Local's |
| `telemt`, `agent/cover`, `agent/disguise`, `agent/supervise`, `agent/logring`, `ctrl` | telemt's config, the cover site check and the process supervision |

### Remote MTProto

The code of Remote MTProto (telemt on remote Linux hosts, managed from the router) is kept
here too and is still compiled and tested, but nothing is built or released from it for
now: `cmd/mtproto-remote`, `cmd/mtproto-agent`, `mux`, `reality`, `realityserver`, `link`,
`tgnet`, `hubnode`, `relayops`, `installer`, `release`, `layout`. Its stages in the
`Dockerfile` are commented out and its release workflows are parked in `.github/remote/`.

## Develop

```sh
make test    # format, vet, go test -race
make lab     # the same tests on Linux, in Docker
make drill   # root-only drills in network namespaces, in Docker

docker buildx build --platform linux/arm64 -t mtproto-local .
```

## Licence

MIT, except `reality/client.go`, a port of xray-core code that stays under
MPL-2.0 (see `reality/LICENSE`).

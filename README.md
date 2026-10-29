# PCP's common items

Ready-made things your [PCP](https://pcp.gg) can take, kept by pcp.gg
(Kaperkunde) in one public place. To start, this holds **services**:
containers PCP can run for you on one of your own SSH servers, with the
Docker or Podman already there, and add as tools your assistants can use
(or, for a VPN, as an exit their web requests can go out through). Skills and API endpoints may follow.

Nothing here is secret, and nothing here ever will be. Every file is plain
text you can read before your PCP does anything with it.

## What is here

| Item | What it is |
| --- | --- |
| [`services/docling.yaml`](services/docling.yaml) | [Docling Serve](https://github.com/docling-project/docling-serve): turns PDFs, Office documents and images into Markdown, JSON or text, with OCR and tables. Runs on CPU; PCP adds its API as an endpoint, sending its key for you. |
| [`services/gluetun.yaml`](services/gluetun.yaml) | [Gluetun](https://github.com/qdm12/gluetun): a VPN client (OpenVPN or WireGuard) for most VPN providers, or a WireGuard server of your own. PCP adds its proxy as an **exit**, so web fetch and the browser can go out through your VPN instead of from your PCP's address. |

### What each one needs

- **Docling**: Docker or Podman on the SSH server, a port PCP can reach,
  and one secret of yours for its API key (`DOCLING_SERVE_API_KEY`). It
  downloads its models on first use, so give it a few minutes and some disk.
- **Gluetun**: Docker or Podman on the SSH server, which must let the
  container have the `NET_ADMIN` capability and the `/dev/net/tun` device
  (the two things a VPN's tunnel needs; PCP gives nothing else). You type
  your provider's name (`VPN_SERVICE_PROVIDER`, as
  [Gluetun names it](https://github.com/qdm12/gluetun-wiki/tree/main/setup/providers)),
  `openvpn` or `wireguard` (`VPN_TYPE`) and, if you like, the countries it
  comes out in (`SERVER_COUNTRIES`); your VPN credentials are secrets of
  yours (`OPENVPN_USER` and `OPENVPN_PASSWORD`, or `WIREGUARD_PRIVATE_KEY`
  and `WIREGUARD_ADDRESSES`). For a WireGuard server of your own, choose
  `custom` and type its address, port and public key. Its proxy is
  published on the SSH server's `127.0.0.1` only, with a password PCP makes
  for it, and PCP reaches it through the SSH connection. You add it on the
  SSH server's page in PCP (**Exits**, then **Add a VPN exit**), and each
  token may use the exit only once you allow it.

A raw WireGuard configuration (what wireproxy would take) goes into Gluetun
as the `custom` provider above: wireproxy reads a configuration file, which
a PCP service has no way to be given.

## Adding it to your PCP

1. In PCP, open **Settings → Repositories** and click **Add PCP's common
   items**. (Or add `kaperkunde/pcp-gg-common` as a read-only repository
   yourself.) No GitHub sign-in is needed: the repository is public.
2. PCP lists what it offers. Choose an item and read it in full: for a
   service, the image it runs, its ports, the variables it takes, its
   volumes, its health check and what PCP adds for it.
3. For a service, **Launch it**: choose your SSH server, where its ports
   are published, and the secret each variable takes (one of yours, by
   name). Nothing runs until you do.

## How it stays yours

- **Read-only and not trusted.** PCP links this repository read-only and
  untrusted. It never pushes here, and nothing here applies by itself.
- **Pinned.** What you add stays exactly as you read it. When something
  changes here, PCP shows it to you as an update, file by file with what it
  would change, and takes it only when you say so.
- **Images by digest.** A service names its image by its digest
  (`…@sha256:…`), so what you approved is what runs. `source` is the tag an
  update looks at; updating is your click, on the service's page in PCP.
- **Definitions only.** A file says what a thing *is*, never where yours
  runs: no SSH server, no address, no secret, no container name. Those are
  yours, chosen in your PCP, and never leave it.
- **No secrets, ever.** A service names its variables; their values are
  your secrets, kept in your PCP and handed to the container over the SSH
  connection only.

## The format

The files follow PCP's own layout for linked repositories, so a PCP reads
them as it reads any repository you link:

```
services/<name>.yaml    a service (pcp_format 3)
README.md, LICENSE, …   nobody's item, left alone
```

A service's document is `pcp_format`, `version`, `name`, `description` and
its definition: `image` (pinned by digest), `source`, `ports`, `env`
(names, with a description, whether it is required, whether you type its
value yourself (`plain`, for what is no secret, like a country), or a fixed
value that is no secret), `volumes` (named volumes, never a folder of your
server), `args`, `cap_add` and `devices` (for an exit only, and only
`NET_ADMIN` and `/dev/net/tun`), `health` and `registers` (an API endpoint
from the schema it serves, an MCP server, an exit, or nothing). A PCP older
than format 3 leaves the
`services/` folder alone.

See [CONTRIBUTING.md](CONTRIBUTING.md) for how an item gets here.

## Licence

The files here are under the [MIT licence](LICENSE). The software a service
runs is under its own licence: Docling Serve is MIT-licensed by its
authors, and Gluetun by its author.

# How an item gets here

pcp.gg keeps this repository. Suggestions are welcome as issues or pull
requests; each one is read by a person before it is merged.

## What an item must be

- **Written by PCP.** Make and test the item in your own PCP, then push it
  to a repository of yours from PCP (its push review shows every file and
  checks it for anything personal). Copy that file here. A file PCP did not
  write may not read back the same, and is not taken.
- **Free of anything personal.** No addresses of your machines, no names,
  no email addresses, no ids from your own setup. PCP's push review flags
  these; fix every hit.
- **No secret, ever.** A variable a service needs is a name; its value is
  each owner's own secret.
- **An image pinned by digest**, from the project's own official registry,
  with `source` naming the tag updates follow. Say in the pull request
  where the digest comes from (the tag and the registry), so a reviewer can
  check it:

  ```sh
  docker buildx imagetools inspect ghcr.io/org/app:1.2.3
  ```

  Where a project publishes no image, or one that cannot run safely as a
  service, the image may instead come from **a fork Kaperkunde keeps** under
  `github.com/kaperkunde/`, built by that fork's own workflow and published
  to its own registry (`ghcr.io/kaperkunde/<name>`). The fork pins one
  upstream release, by tag and commit, and records it with what it changed
  and what its review found (as `KAPERKUNDE.md` does in
  [kaperkunde/pdf-mcp-server](https://github.com/kaperkunde/pdf-mcp-server)).
  The pull request names the upstream tag and commit, the fork's tag, and
  the run of its workflow that printed the digest.

- **Named volumes only**, and only where the service keeps data it needs
  across restarts.
- **A health check** where the service has one, and `registers` set to what
  PCP should add for it.
- **No privileges but a VPN's.** Only an exit (`registers` kind `exit`) may
  ask for `cap_add: [NET_ADMIN]` and `devices: [/dev/net/tun]`, and PCP
  takes nothing else. Its proxy takes the login PCP makes (`auth`), never
  one written here.

## What a reviewer checks

1. The image's digest is the one the official registry gives for the tag
   named, or, for a Kaperkunde fork's image, the one its workflow run
   printed for the fork's tag, built from the upstream commit recorded
   there.
2. The ports, variables, volumes and arguments are what the project's own
   documentation says, and nothing more.
3. The file reads back in PCP unchanged (add this repository to a PCP and
   look at the item).
4. Nothing personal and nothing secret is in it.

An update (a new digest, a new variable) is a pull request like any other.
Every PCP that added the item sees it as an update and takes it only when
its owner says so.

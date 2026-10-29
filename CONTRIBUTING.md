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

- **Named volumes only**, and only where the service keeps data it needs
  across restarts.
- **A health check** where the service has one, and `registers` set to what
  PCP should add for it.

## What a reviewer checks

1. The image's digest is the one the official registry gives for the tag
   named.
2. The ports, variables, volumes and arguments are what the project's own
   documentation says, and nothing more.
3. The file reads back in PCP unchanged (add this repository to a PCP and
   look at the item).
4. Nothing personal and nothing secret is in it.

An update (a new digest, a new variable) is a pull request like any other.
Every PCP that added the item sees it as an update and takes it only when
its owner says so.

# Introduction

This repo contains a simple Docker Compose configuration that runs an sftp
server on your tailnet with Tailscale, intended primarily for syncing
Keepass databases.

# Setup

## Environment

Copy `.env.sample` in the base directory to `.env` and edit the environment
variables defined in there to suit your configuration.

Copy `./config/sftp.json.sample` to `./config/sftp.json` and add users and
passwords.

## Register with Tailscale

Bring the stack up with `docker-compose up -d` then find out the name of the
tailscale container by running `docker ps`. For example:

```
CONTAINER ID   IMAGE                        COMMAND                  CREATED          STATUS          PORTS     NAMES
keepass-sftp-sftp-1        emberstack/sftp              "tini -- dotnet ES.S…"   sftp        16 minutes ago   Up 3 seconds
keepass-sftp-tailscale-1   tailscale/tailscale:latest   "/usr/local/bin/cont…"   tailscale   19 minutes ago   Up 3 seconds
```

Register the tailscale container with Tailscale manually by running:

```
docker exec keepass-sftp-tailscale-1 tailscale up
```

This should display a URL that you can navigate to and complete the registration
to your tailnet:

```
To authenticate, visit:

        https://login.tailscale.com/a/nnnnnnnnnnnnnn
```

Leave the `docker exec` command running while you complete the registration. It
will exit back to the shell afterwards.

# Testing

At this point you should be able to sftp to the tailscale node on your tailnet
using the credentials that you added to `sftp.json`.

If there are any issues check the logs for the Tailscale and SFTP containers by
running:

```
docker logs -f keepass-sftp-tailscale-1
docker logs -f keepass-sftp-sftp-1
```

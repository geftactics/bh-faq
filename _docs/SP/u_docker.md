---
title: Docker
subtitle:
tags: []
author: greenknot
layout: doc_sp
---

#### Sections in this article
{:.no_toc}
* TOC
{:toc}

## How to use the Docker image

A docker image is available on [Docker Hub](https://hub.docker.com/r/ledgerhq/speculos). Pull the latest image:

```
docker pull ledgerhq/speculos
```

And run the image with a few arguments from the root of the speculos project:

```
docker run -v $(pwd)/apps:/speculos/apps --publish 5900:5900 -it ledgerhq/speculos --display headless --vnc-port 5900 apps/btc.elf
```

- The app folder (here `$(pwd)/apps/`) is mounted thanks to `-v`
- The VNC server is available from the host thanks to `--publish`

The image can obviously run an interactive shell with `--entrypoint /bin/bash`.


### Arguments

All the arguments which are supported by `speculos.py` can be passed on the Docker command-line. Don't forget to publish container's ports when required using `-p`:

```console
docker run -it -v "$(pwd)"/apps:/speculos/apps \
-p 1234:1234 -p 40000:40000 -p 41000:41000 -p 42000:42000 \
ledgerhq/speculos --model nanos ./apps/btc.elf --sdk 1.6 --seed "secret" --display headless \
--apdu-port 40000 --vnc-port 41000 --button-port 42000
```

### Debug

```console
docker run -it -v "$(pwd)"/apps:/speculos/apps -p 1234:1234 -p 40000:40000 -p 41000:41000 -p 42000:42000 --entrypoint /bin/bash ledgerhq/speculos
```

### docker-compose setup

```console
docker-compose up [-d]
```
> Default configuration is nanos / 1.6 / btc.elf / seed "secret"

Edit `docker-compose.yml` to configure port forwarding and environment variables that fit your needs.

## Build

The following command-line can be used to create a docker image based on a local
[build](../i_build.md):

```console
docker build ./ -t speculos
```

Replace `ledgerhq/speculos` with `speculos` in the sections above to use this
image.

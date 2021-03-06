# SKS Key Server

![](https://images.microbadger.com/badges/image/zhusj/sks.svg)

## Intro

This is [SKS Key Server](https://bitbucket.org/skskeyserver/sks-keyserver) unofficial Dockerfile.
Currently the latest version is 1.1.6.

## Guide

This container has a volume which is `/var/lib/sks` inside. Please mount it for persistent storage.

When you run this container first time, please put a key dump in dir `/var/lib/sks/dump`.
And run `docker run --rm -v /var/lib/sks/:/var/lib/sks/ zhusj/sks sks-init`.

After that, you can start the container with:

```
docker run -it -d --restart=always --name sks-keyserver -v /var/lib/sks/:/var/lib/sks/ \
  -p 0.0.0.0:11370:11370/tcp -p 127.0.0.1:11371:11371/tcp zhusj/sks
```

Besides you should setup a reverse proxy server for `127.0.0.1:11371`.

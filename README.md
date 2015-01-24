# docker-alpine

[![CircleCI](https://img.shields.io/circleci/project/gliderlabs/docker-alpine.svg)](https://circleci.com/gh/gliderlabs/docker-alpine)

A super small Docker image based on Alpine Linux. The image is only 5 MB and has access to a package repository that is much more complete than other BusyBox based images.

## Why?

Docker images today are big. Usually much larger than they need to be. There are a lot of ways to make them smaller. But the Docker populous still jumps to the `ubuntu` base image for most projects. The size savings over `ubuntu` and other bases are huge:

```
REPOSITORY          TAG           IMAGE ID          VIRTUAL SIZE
gliderlabs/alpine   latest        157314031a17      5.03 MB
debian              latest        4d6ce913b130      84.98 MB
ubuntu              latest        b39b81afc8ca      188.3 MB
centos              latest        8efe422e6104      210 MB
```

There are images such as `progrium/busybox` which get us very close to a minimal container and package system. But these particular BusyBox builds piggyback on the OpenWRT package index which is often lacking and not tailored towards generic everyday applications. Alpine Linux has a much more complete and update to date [package index][alpine-packages]:

```console
$ docker run progrium/busybox opkg-install nodejs
Unknown package 'nodejs'.
Collected errors:
* opkg_install_cmd: Cannot install package nodejs.

$ docker run gliderlabs/alpine apk-install nodejs
fetch http://dl-4.alpinelinux.org/alpine/v3.1/main/x86_64/APKINDEX.tar.gz
(1/5) Installing c-ares (1.10.0-r1)
(2/5) Installing libgcc (4.8.3-r0)
(3/5) Installing libstdc++ (4.8.3-r0)
(4/5) Installing libuv (0.10.29-r0)
(5/5) Installing nodejs (0.10.33-r0)
Executing busybox-1.22.1-r14.trigger
OK: 21 MiB in 20 packages
```

This makes Alpine Linux a great image base for utilities and even production applications. [Read more about Alpine Linux here][alpine-about] and you can see how their mantra fits in right at home with Docker images.

## Build

Each build has a version file in the `versions` folder. The file name is the Alpine Linux release to build and the contents of the file are the image tags to apply once built (one tag per line).

Use the `build` sub-command of the `alpine` utility to build the images:

```console
$ ./alpine build
-----> Building Alpine Linux Docker images ...
       Building image for the rootfs mkimage script ... done
       Building release edge ... done
       Tagging release edge as alpine:edge
=====> Finished building release edge
       Building release v3.1 ... done
       Tagging release v3.1 as alpine:3.1
       Tagging release v3.1 as alpine:latest
=====> Finished building release v3.1
```

The build utility takes a list of files or glob as an argument. You could build just the edge version using `./alpine build versions/edge`.

## Test

The test for images is very simple at the moment. It just attempts to install the `openssl` package and verify we exit cleanly.

Use the `test` sub-command of the `alpine` utility to run tests on currently build images:

```console
$ ./alpine test
-----> Testing images ...
       Testing image alpine:edge ... passed
       Testing image alpine:3.1 ... passed
```

## Inspiration

The motivation for this project and modifications to `mkimage.sh` are highly inspired by Eivind Uggedal (uggedal) and Luis Lavena (luislavena). They have made great strides in getting Alpine Linux running as a Docker container. Check out their [mini-container/base][mini-base] image as well.

[mini-base]: https://github.com/mini-containers/base
[alpine-packages]: http://forum.alpinelinux.org/packages
[alpine-about]: https://www.alpinelinux.org/about/
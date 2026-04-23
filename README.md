# BusyBox rock

This repository contains the [rock](https://documentation.ubuntu.com/server/explanation/virtualisation/about-rock-images/) definitions for [BusyBox](https://busybox.net/), a software suite that provides several Unix utilities in a single executable.

The rock ships the `busybox` binary together with symlinks for every applet it exposes (`sh`, `ls`, `cat`, `wget`, `sed`, `awk`, ...), making it a small base for shell scripting, debugging, or multi-stage image builds.

## Example

Let's run a shell inside the rock and poke around.

```bash
$ docker run --name=my-busybox-rock --rm -it busybox:1.37
2025-12-17T16:33:37.340Z [pebble] {"type":"security","datetime":"2025-12-17T16:33:37Z","level":"WARN","event":"sys_startup:0","description":"Starting daemon","appid":"pebble"}
2025-12-17T16:33:37.341Z [pebble] Started daemon.
2025-12-17T16:33:37.341Z [pebble] POST /v1/services 78.436µs 400 (http+unix)
2025-12-17T16:33:37.341Z [pebble] Cannot start default services: no default services
```

The rock is running under [`pebble`](https://github.com/canonical/pebble) with no default services. This is expected - BusyBox is not a service rock, it's a utility toolbox. In a separate terminal, exec in:

```bash
docker exec -it my-busybox-rock sh
```

List the applets BusyBox provides:

```bash
busybox --list | head
```

Or use any of them directly via the symlinks:

```bash
$ ls -l /etc/os-release
lrwxrwxrwx    1 root     root            21 Jan  1  1970 /etc/os-release -> ../usr/lib/os-release
$ wget -qO- https://example.com | head -n 1
<!doctype html>
```

Voilà.

## Available versions

* [BusyBox 1.37 (Ubuntu 26.04)](./busybox/1.37-26.04/rockcraft.yaml)

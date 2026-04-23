# BusyBox rock

This repository contains the [rock](https://documentation.ubuntu.com/server/explanation/virtualisation/about-rock-images/) definitions for [BusyBox](https://busybox.net/), a software suite that provides several Unix utilities in a single executable.

The rock ships the `busybox` binary together with symlinks for every applet it exposes (`sh`, `ls`, `cat`, `wget`, `sed`, `awk`, ...), making it a small base for shell scripting, debugging, or multi-stage image builds.

## Example

Let's run a shell inside the rock and poke around.

```bash
$ docker run --name=my-busybox-rock --rm busybox:1.37
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
$ ls -lha /etc | head
total 56K
drwxr-xr-x    1 0        0           4.0K Apr 23 12:43 .
dr-xr-xr-x    1 0        0           4.0K Apr 23 12:43 ..
-rw-r--r--    1 0        0             10 Apr 23 12:41 debian_version
drwxr-xr-x    3 0        0           4.0K Apr 23 12:41 dpkg
-rw-r--r--    1 0        0             19 Apr 23 12:41 group
-rw-r--r--    1 0        0             92 Apr 23 12:41 host.conf
-rw-r--r--    1 0        0             13 Apr 23 12:43 hostname
-rw-r--r--    1 0        0            266 Apr 23 12:43 hosts
-rw-r--r--    1 0        0             24 Apr 23 12:41 issue
```

```bash
$ wget -qO- http://archive.ubuntu.com | head -n1
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
```

Voilà.

## Available versions

* [BusyBox 1.37 (Ubuntu 26.04)](./busybox/1.37-26.04/rockcraft.yaml)

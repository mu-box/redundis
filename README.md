[![redundis logo](http://microbox.rocks/assets/readme-headers/redundis.png)](http://microbox.cloud/open-source#redundis)
[![Build Status](https://github.com/mu-box/redundis/actions/workflows/ci.yaml/badge.svg)](https://github.com/mu-box/redundis/actions)
[![GoDoc](https://godoc.org/github.com/mu-box/redundis?status.svg)](https://godoc.org/github.com/mu-box/redundis)

# Redundis

Redis high-availability cluster using Sentinel to transparently proxy connections to the active primary member - with full redis capability.

Redundis is a smart, sentinel aware proxy for redis that allows redis clients to not care about failover of the redis master node.

Connections are automatically forwarded to the master redis node, and when the master node fails over, clients are disconnected and reconnected to the new master node.

## Status

Complete/Stable

## Quickstart

Simply run `redundis` to start redundis with the default settings or `redundis -c config.json` to use config from a file.

## Config

Redundis can be configured via a config file, or command line flags. Any configuration in a specified config file will overwrite any cli flags.

### Config Flags

`redundis -h` will show usage and a list of flags:

```
redundis redis-proxy

Usage:
  redundis [flags]

Flags:
  -c, --config-file="": Config file location for redundis
  -l, --listen-address="127.0.0.1:6379": Redundis listen address
  -L, --log-level="info": Log level [fatal, error, info, debug, trace]
  -t, --master-wait=30: Time to wait for node to transition to master (seconds)
  -m, --monitor-name="test": Name of sentinel monitor
  -r, --ready-wait=30: Time to wait to connect to redis|sentinel (seconds)
  -s, --sentinel-address="127.0.0.1:26379": Address of sentinel node
  -p, --sentinel-password="": Sentinel password
  -w, --sentinel-wait=10: Time to wait for sentinel to respond (seconds)
```

### Config File

You can specify a config file by using `redundis -c config.json`
```json
{
  "listen-address": "127.0.0.1:6379",
  "sentinel-address": "127.0.0.1:26379",
  "sentinel-password": "",
  "monitor-name": "test",
  "master-wait": 30,
  "ready-wait": 30,
  "sentinel-wait": 10,
  "log-level": "info"
}
```

## Redis Setup

Each redis node will be configured as follows:
```
+----------+
| redis    | - redis-server running on port 6380 with sentinel configured
| sentinel | - sentinel configured and started (default listen port of 26379)
| redundis | - proxies incoming connections to master redis (determined by talking to local sentinel)
| flip*    | - manages cluster vip that users connect to
+----------+
* = optional
```

## Limitations

Currently, only connecting to one sentinel is supported. It could be extended in the future to connect to a different sentinel incase of sentinel failure, but right now this is not needed.

## Todo

- Handle network partitions

[![redundis logo](http://microbox.rocks/assets/open-src/microbox-open-src.png)](http://microbox.cloud/open-source)

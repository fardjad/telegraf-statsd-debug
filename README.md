# StatsD Debugging with Telegraf

This repository contains a simple setup to debug StatsD metrics using
[Telegraf](https://github.com/influxdata/telegraf).

## Usage

1. Make sure you have Docker and Docker Compose installed.
2. Start the Telegraf container with `docker-compose up`.
3. Set the StatsD client host and port in your application and run it.
4. View the metrics in the Telegraf container logs.

_Note: To test the Telegraf container/config, you can send a metric to Telegraf
using netcat:_

```sh
echo "mycounter:10|c" | nc -C -w 1 -u localhost 8125 # UDP
echo "mycounter:10|c" | nc -C -w 1 localhost 8125 # TCP
```

## Custom Telegraf Configuration

To generate a Telegraf config file, run:

```sh
docker run --rm -it telegraf --config > telegraf.conf
```

The file can be quite large and contains many comments. To see the effective
configuration, run:

```sh
cat telegraf.conf | grep -Ev '^\s*#' | grep -v '^[[:space:]]*$'
```

After generating the config file, you can modify it to suit your needs. The
[file](./telegraf.conf) in this repository is a minimal configuration to listen
for StatsD metrics on port 8125 (both UDP and TCP) and print them to the stdout.

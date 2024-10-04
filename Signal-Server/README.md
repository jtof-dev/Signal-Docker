# Signal-Server Dockerized!

The source files running Signal-Server in a docker container. At the moment, Signal-Server expects to be ran in an EC2 container, so this is the skeleton of what a Signal-Server could look like. Most likely, this Docker image needs to be rewritten to use [metadataproxy](https://github.com/lyft/metadataproxy), which I don't plan on doing.

## Compilation

`cd Signal-Server`

Create a `signal-server` Docker image:

```
docker build --no-cache -t signal-server:1.0 .
```

If you need to reinstall, first run `docker rmi -f signal-server:1.0`

Generate the correct cluster volumes with `bash docker-compose-first-run.sh`

If you call the main `docker-compose.yml` instead of `docker-compose-first-run.yml`, the server will fail with an error related to not being able to connect to the redis cluster

You can fix this by listing all volumes and deleting the ones you just generated:

```
docker volume ls

docker volume rm -f <name-1>
docker volume rm -f <name-2>
etc
```

## Configuration

- Folllow [/signal-server-configuration.md` from Main](https://github.com/jtof-dev/Signal-Server/blob/main/docs/signal-server-configuration.md), and make sure to also follow the [Docker configuration](https://github.com/jtof-dev/Signal-Server/blob/main/docs/signal-server-configuration.md#dockerized-signal-server-documentation)

- Place your completed `config.yml` and `config-secrets-bundle.yml` in `personal-config/`

## Starting the container

Start the server:

```
docker compose up
```

### Starting the container with [`filtered-docker-compose.sh`](filtered-docker-compose.sh)

This script just calls a one-liner `docker-compose up --no-log-prefix` and runs it through some `awk` / `sed` filters

- Currently the long datadog failed html output is the only thing omitted (since it throws 100 lines of code every couple of seconds and provides no useful info)

- Also colors the words `INFO`, `WARN`, and `ERROR` to green, orange, and red respectively to make it easier to read the server's logs

# To Do

## Signal-Server

- Use [this EC2 spoofer tool](https://github.com/lyft/metadataproxy) to make the docker container work

- Rewrite the docker container to just build a server, and have another java image run it

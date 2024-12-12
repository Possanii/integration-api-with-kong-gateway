## How to use this Compose file

Kong Gateway can be deployed in different ways. This Docker Compose file provides
support for running Kong in [db-less][kong-docs-dbless] mode, in which only a Kong
container is spun up, or with a backing database. The default is db-less mode:

```shell
$ docker compose up -d
```

This command will result in a single Kong Docker container:

```shell
$ docker ps
NAME                IMAGE               COMMAND                  SERVICE             CREATED             STATUS                    PORTS
compose-kong-1      kong:latest         "/docker-entrypoint.…"   kong                38 seconds ago      Up 29 seconds (healthy)   0.0.0.0:8000->8000/tcp, 127.0.0.1:8001->8001/tcp, 0.0.0.0:8443->8443/tcp, 127.0.0.1:8444->8444/tcp
```

You can also run Kong with a backing Postgres database:

```shell
$ KONG_DATABASE=postgres docker compose --profile database up -d
```

Which will result in two Docker containers running -- one for Kong itself, and
another for the Postgres instance it uses to store its configuration entities:

```shell
$ docker compose ps
NAME                IMAGE               COMMAND                  SERVICE             CREATED              STATUS                        PORTS
compose-db-1        postgres:9.5        "docker-entrypoint.s…"   db                  About a minute ago   Up About a minute (healthy)   5432/tcp
compose-kong-1      kong:latest         "/docker-entrypoint.…"   kong                About a minute ago   Up About a minute (healthy)   0.0.0.0:8000->8000/tcp, 127.0.0.1:8001->8001/tcp, 0.0.0.0:8443->8443/tcp, 127.0.0.1:8444->8444/tcp
```

Kong will be available on port `8000` and `8001`. You can customize the template
with your own environment variables or datastore configuration.

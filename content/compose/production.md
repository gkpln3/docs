---
description: Guide to using Docker Compose in production
keywords: compose, orchestration, containers, production
title: Use Compose in production
---

When you define your app with Compose in development, you can use this
definition to run your application in different environments such as CI,
staging, and production.

The easiest way to deploy an application is to run it on a single server,
similar to how you would run your development environment. If you want to scale
up your application, you can run Compose apps on a Swarm cluster.

### Modify your Compose file for production

You may need to make changes to your app configuration to make it ready for
production. These changes might include:

- Removing any volume bindings for application code, so that code stays inside
  the container and can't be changed from outside
- Binding to different ports on the host
- Setting environment variables differently, such as reducing the verbosity of
  logging, or to specify settings for external services such as an email server
- Specifying a restart policy like [`restart: always`](compose-file/index.md#restart){: target="_blank" rel="noopener" class="_" } to avoid downtime
- Adding extra services such as a log aggregator

For this reason, consider defining an additional Compose file, say
`production.yml`, which specifies production-appropriate
configuration. This configuration file only needs to include the changes you want to make from the original Compose file. The additional Compose file
is then applied over the original `docker-compose.yml` to create a new configuration.

Once you have a second configuration file, you can use it with the
`-f` option:

```console
$ docker compose -f docker-compose.yml -f production.yml up -d
```

See [Using multiple compose files](extends.md#different-environments) for a more
complete example.

### Deploying changes

When you make changes to your app code, remember to rebuild your image and
recreate your app's containers. To redeploy a service called
`web`, use:

```console
$ docker compose build web
$ docker compose up --no-deps -d web
```

This first command rebuilds the image for `web` and then stops, destroys, and recreates
just the `web` service. The `--no-deps` flag prevents Compose from also
recreating any services which `web` depends on.

### Running Compose on a single server

You can use Compose to deploy an app to a remote Docker host by setting the
`DOCKER_HOST`, `DOCKER_TLS_VERIFY`, and `DOCKER_CERT_PATH` environment variables
appropriately. See also [Compose CLI environment variables](reference/envvars.md).

Once you've set up your environment variables, all the normal `docker compose`
commands work with no further configuration.

## What's new?

- [Command line reference](reference/index.md)
- [Compose file reference](compose-file/index.md)
- [Sample apps with Compose](samples-for-compose.md)
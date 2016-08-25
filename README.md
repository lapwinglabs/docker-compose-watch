# docker-compose-watch
simple bash bash-based `docker-compose` wrapper that restarts services when any files in its host volumes change

This script has a few dependencies and currently assumes a **mac environment with Docker for Mac, docker-compose 1.8+ and compose files following the v2 spec**.  This would probably port easily to linux but I have no use for that right now. If you're on mac and don't have the proper dependencies installed, the script will tell you what you're missing and how to get it.

## Installation

Assuming /usr/local/bin is on your path...
```
curl -Ls https://raw.githubusercontent.com/lapwinglabs/docker-compose-watch/master/docker-compose-watch > /usr/local/bin/docker-compose-watch && chmod +x /usr/local/bin/docker-compose-watch```
> Protip: add `alias dcw="docker-compose-watch"` to your bash profile for more convenient awesomeness

## Usage
From the directory of your compose project:

```
docker-compose-watch [-l|--log <service>] [service1] [service2] ...
```
If no services are specified, docker-compose will bring up all services defined in the compose file

### Options
`-l|--log <service>`

> Tails logs for only the selected service. Can be specified multiple times for tailing multiple service logs. Primarily useful for allowing docker-compose to manage sidekick services while excluding them from the log output.

## What it does
This script simply sets up a file watch on any host volumes specified for services defined in the docker-compose file.

When you run `docker-compose-watch`, it will bring up services defined in `docker-compose.yml` of your current directory using `docker-compose`. If any files in a host volume are modified, any compose service using that volume will be restarted (this doesn't support `volumes_from` yet), and logging will be reattached.

When you kill the script, it uses `docker-compose` to stop all services specified on startup.

## Things I'll probably need soon...
- [ ] support for `volumes_from`
- [ ] specification of alternate docker-compose file
- [ ] watching for changes to docker-compose file itself and re-up all services on change
- [ ] support for watching the build context of services and rebuilding service image on change

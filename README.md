# Vault Docker

This repo covers simple and typical configurations used to run the [Official Vault Docker image](https://registry.hub.docker.com/_/vault/).

## Configuration

The preferred method to configure Vault is via running `docker-compose` and using
the `docker-compose.yml` file.  More advanced configurations--especially non-\'dev\' instances--requiring certificates, mounted volumes, etc. are detailed on other branches.

## Running

Two shorthand scripts found in `bin` indicate how the image is run.

## CLI

It is assumed that the [Vault CLI](https://www.vaultproject.io/downloads) might be used to communicate with the Vault container and that the CLI is run outside of Docker.

In "dev mode", the Vault server is not using HTTPS. The Vault CLI will try to use HTTPS by default, resulting in an error. You can fix this by running the CLI with the `VAULT_ADDR` environment variable set.  The default address/URL is "http://127.0.0.0:8200"

## Files

1. `docker-compose.override.yml` contains non-generic, local configuration.
2. `private/credentials` The "private" directory should not be included in the git repo.

## Set Up

1. Create the private/credentials file and set the permissions so others can not read or write the directory/file.  The credentials file is sourced by `docker-compose` and the value is used in `docker-compose.override.yml`. Set the desired E_VAULT_DEV_ROOT_TOKEN_ID value. For example:
```
E_VAULT_DEV_ROOT_TOKEN_ID="myroottoken"
```

# Vault Docker

This repo covers simple and typical configurations used to run the [Official Vault Docker image](https://registry.hub.docker.com/_/vault/).

## Configuration

The preferred method to configure Vault is via running `docker-compose` and using
the `docker-compose.yml` file.  More advanced configurations--especially non-\'dev\' instances--requiring certificates, mounted volumes, etc. are detailed on other branches.

## Running

Two shorthand scripts found in `bin` indicate how the image is run.

When the Vault server comes up, it will be sealed.  Before anything can be done to the server, it must be unsealed.  The Vault CLI needs to be run to [unseal the vault](https://learn.hashicorp.com/tutorials/vault/getting-started-deploy?in=vault/getting-started#initializing-the-vault).

## CLI

It is assumed that the [Vault CLI](https://www.vaultproject.io/downloads) might be used to communicate with the Vault container and that the CLI is run outside of Docker.

The Vault CLI will try to use HTTPS by default, resulting in an error if the Vault server is not using tls. You can fix this by running the CLI with the `VAULT_ADDR` environment variable set.  The default address/URL is "http://127.0.0.0:8200"

## Files

1. The `volumes` directory contains filesystems mounted for the Vault server.  Except for the `config` directory, these are excluded from the repo.

## Set Up

1. Run `bin/init` to create the volumes that will be mounted by the Vault server.


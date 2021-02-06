# Vault Docker

This repo covers simple and typical configurations used to run the [Official Vault Docker image](https://registry.hub.docker.com/_/vault/).

## Configuration

The preferred method to configure Vault is via running `docker-compose` and using
the `docker-compose.yml` file.  More advanced configurations--especially non-\'dev\' instances--requiring certificates, mounted volumes, etc. are detailed on other branches.

## Running

Three shorthand scripts found in `bin` indicate how the image is run.

## CLI

It is assumed that the [Vault CLI](https://www.vaultproject.io/downloads) might be used to communicate with the Vault container and that the CLI is run outside of Docker.

In "dev mode", the Vault server is not using HTTPS. The Vault CLI will try to use HTTPS by default, resulting in an error. You can fix this by running the CLI with the `VAULT_ADDR` environment variable set.  The default address/URL is "http://127.0.0.0:820"

## Files

In this default configuration running a Vault dev server, there are no additional files.

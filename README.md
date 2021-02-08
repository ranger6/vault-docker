# Vault Docker

This repo covers simple and typical configurations used to run the [Official Vault Docker image](https://registry.hub.docker.com/_/vault/).

One major motivation for creating this repo was for use with the Concourse CI server. A
[companion project](https://github.com/ranger6/concourse-docker) describes setting up and configuring Concourse using Vault.

## Configuration

The preferred method to configure Vault is via running `docker-compose` and using
the `docker-compose.yml` file.  More advanced configurations--especially non-\'dev\' instances--requiring certificates, mounted volumes, etc. are detailed on other branches.

When the Vault server is using tls, it needs its own certificate and key.  Generally, clients
will need the Certificate Authority certificate that signed the Vault server certificate. One
key client is the Vault CLI.  This (according to the Vault documentation) may be provided by
the host operating system.  Or, it can be provided via the VAULT_CACERT environment variable.

## Running

Two shorthand scripts found in `bin` indicate how the image is run.

When the Vault server comes up, it will be sealed.  Before anything can be done to the server, it must be unsealed.  The Vault CLI needs to be run to [unseal the vault](https://learn.hashicorp.com/tutorials/vault/getting-started-deploy?in=vault/getting-started#initializing-the-vault).

## CLI

It is assumed that the [Vault CLI](https://www.vaultproject.io/downloads) used to communicate with the Vault container will be run outside of Docker.  It is possible to run the CLI in a container, but the configuration for this case is not covered here.

## Files

1. The `volumes` directory contains filesystems mounted for the Vault server.  Except for the `config` directory, these are excluded from the repo. See `docker-compose.yml` for how the following volumes are mounted in the Vault container:
    1.  `volumes/file` is the file system Vault uses to persist data.
    2.  `volumes/config` contains configuration files
    3.  `volumes/tls` contains certificates

2. The Vault config file is found in `volumes/config/vault.json`    

3. The minimum certificates needed to enable tls are:
    1.  The vault server certificate: `volumes/tls/vault.crt`
    2.  The vault certificate key: `volumes/tls/vault.key`

## Set Up

1. Run `bin/init` to create the volumes that will be mounted by the Vault server.
2. Generate the Vault certificates: CA and server certificate with keys (with no passphrase)
and place them in `volumes/tls`.
Helpful documentation includes the [Example](https://concourse-ci.org/vault-credential-manager.html#vault-cert-auth)
at the end this Concourse documentation section.
A script using [certstrap](https://github.com/square/certstrap) to generate certificates
is found in the `bin` directory.  Move `vault.crt` and `vault.key` to `volumes/tls`.
Note that the CA certificate is not needed by the Vault server, but *will* be needed by clients.
Clients will be more or less picky about the Vault certificate
(CLI, Browsers, API clients). Common names, alternate subject names,
IP addresses in the certificate (and in the CA certificate?) may make a difference.
For example, the Concourse client requires that the certificate contain the name used as
the Vault host in the API URL. The E_VAULT_HOSTNAME variable in the `gencerts` script can
be set to the same value that Concourse uses to access Vault.

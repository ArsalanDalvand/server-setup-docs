---
title: "mc idp openid — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-idp-openid.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-idp-openid"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc idp openid — SILO (MinIO Object Storage)"
---
# mc idp openid

New in version RELEASE.2023-05-26T23-31-54Z: [`mc idp openid`](#command-mc.idp.openid) and its subcommands replace `mc admin idp openid`.

## Description

The [`mc idp openid`](#command-mc.idp.openid) commands allow you to manage configurations to 3rd party [[administration/identity-access-management/oidc-access-management#minio-external-identity-management-openid|OpenID Identity and Access Management (IAM) integrations]].

Define configuration settings as an alternative to using environment variables when [[operations/external-iam/configure-openid-external-identity-management#minio-external-identity-management-openid-configure|setting up an OpenID connection]]. The [`mc idp openid`](#command-mc.idp.openid) commands are only supported against MinIO deployments.

Note

MinIO [[reference/minio-server/settings/iam/openid#minio-server-envvar-external-identity-management-openid|OpenID environment variables]] override their corresponding configuration settings as modified or set by this command.

The [`mc idp openid`](#command-mc.idp.openid) command has the following subcommands:

Subcommand

Description

[`mc idp openid add`](#mc.idp.openid.add)

Create an OpenID IDP server configuration.

[`mc idp openid update`](#mc.idp.openid.update)

Modify an existing OpenID IDP server configuration.

[`mc idp openid rm`](#mc.idp.openid.rm)

Remove an OpenID IDP server configuration from a deployment.

[`mc idp openid ls`](#mc.idp.openid.ls)

Outputs a list of the existing OpenID server configurations for a deployment.

[`mc idp openid info`](#mc.idp.openid.info)

Displays details for a specific OpenID server configuration.

[`mc idp openid enable`](#mc.idp.openid.enable)

Enables an OpenID server configuration.

[`mc idp openid disable`](#mc.idp.openid.disable)

Disables an OpenID server configuration.

## Configuration Parameters

The [`mc idp openid`](#command-mc.idp.openid) subcommands support configuration parameters.
The parameters define the server’s interaction with the IAM provider.

For a more detailed explanation of the configuration parameters, refer to the [[reference/minio-server/settings/iam/openid#minio-open-id-config-settings|config setting documentation]].

## Syntax

add[](#mc.idp.openid.add)

Create a new set of configurations for an OpenID provider.

You can run the command multiple times to set up multiple OpenID providers.

When adding multiple OpenID providers, only one can be a JWT Claim-based provider.
All others must be role-based providers.

EXAMPLE

The following example creates the configuration settings for the `myminio` deployment as defined in a new `test-config` setup for Dex integration.

```
mc idp openid add myminio test-config                                        \
    client_id=minio-client-app                                                \
    client_secret=minio-client-app-secret                                     \
    config_url="http://localhost:5556/dex/.well-known/openid-configuration"   \
    scopes="openid,groups"                                                    \
    redirect_uri="http://127.0.0.1:10000/oauth_callback"                      \
    role_policy="consoleAdmin"
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] idp openid add               \
                            ALIAS             \
                            [CFG_NAME]        \
                            [CFG_PARAM1]      \
                            [CFG_PARAM2]...
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment to configure for OpenID integration.

Replace `CFG_NAME` with a unique string for this configuration.
If not specified, the command creates default configuration values.

Replace the `[CFG_PARAM#]` with each of the [[reference/minio-server/settings/iam/openid#minio-open-id-config-settings|configuration setting]] key-value pairs in the format of `PARAMETER="value"`.

update[](#mc.idp.openid.update)

Modify an existing set of configurations for an OpenID provider.

EXAMPLE

The following example changes two of the configuration settings for the `myminio` deployment as defined in the `test-config` setup for Dex integration.

```
mc idp openid update                      \
              myminio                     \
              test_config                 \
              scopes="openid,groups"      \
              role_policy="consoleAdmin"
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] idp openid update           \
                            ALIAS            \
                            [CFG_NAME]       \
                            [CFG_PARAM1]     \
                            [CFG_PARAM2]...
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment to configure for OpenID integration.

Replace `CFG_NAME` with a unique string for this configuration.
If not specified, the command updates the default configuration.

Replace the `[CFG_PARAM#]` with each of the [[reference/minio-server/settings/iam/openid#minio-open-id-config-settings|configuration setting]] key-value pairs to update in the format of `PARAMETER="value"`.

rm, remove[](#mc.idp.openid.rm)

Remove an existing set of configurations for an OpenID provider.

EXAMPLE

The following example removes the `test-config` settings for the `myminio` deployment.

```
mc idp openid rm myminio test_config
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] idp openid rm          \
                            ALIAS       \
                            [CFG_NAME]
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment to configure for OpenID integration.

Replace `CFG_NAME` with a unique string for this configuration.
If not specified, the command removes the default configurations.

ls, list[](#mc.idp.openid.ls)

Outputs a list of existing configuration sets for OpenID providers.

EXAMPLE

The following example outputs a list of all OpenID configuration sets defined for the `myminio` deployment.

```
mc idp openid ls myminio
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] idp openid ls ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment to list OpenID integrations for.

info[](#mc.idp.openid.info)

Outputs the set of values defined for an existing set of server configurations for an OpenID provider.

EXAMPLE

The following example outputs the configuration settings defined for the `test_config` set of OpenID settings on the `myminio` deployment.

```
mc idp openid info myminio test_config
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] idp openid info        \
                            ALIAS       \
                            [CFG_NAME]
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment to configure for OpenID integration.

Replace `CFG_NAME` with a unique string for this configuration.
If not specified, the information displays for the default server configuration.

enable[](#mc.idp.openid.enable)

Begin using an existing set of configurations for an OpenID provider.

EXAMPLE

The following example enables the server configurations defined as `test_config` on the `myminio` deployment.

```
mc idp openid enable       \
              myminio      \
              test_config
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] idp openid enable     \
                            ALIAS      \
                            [CFG_NAME]
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment to configure for OpenID integration.

Replace `CFG_NAME` with a unique string for this configuration.
If not specified, the command enables the default configuration values.

disable[](#mc.idp.openid.disable)

Stop using a set of configurations for an OpenID provider.

EXAMPLE

The following example disables the server configurations defined as `test_config` on the `myminio` deployment.

```
mc idp openid disable      \
              myminio      \
              test_config
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] idp openid disable       \
                            ALIAS         \
                            [CFG_NAME]
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment to configure for OpenID integration.

Replace `CFG_NAME` with a unique string for this configuration.
If not specified, the command disables the default configuration values.

## Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).
---
title: "Transport Layer Security (TLS)"
source: "https://redis.io/docs/latest/operate/rc/security/database-security/tls-ssl/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/security/database-security/tls-ssl"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Transport Layer Security (TLS)"
---
# Transport Layer Security (TLS)

Enable TLS to encrypt data communications between applications and Redis databases.

            Redis Cloud
          

Transport Layer Security (TLS) uses encryption to secure [network communications](https://en.wikipedia.org/wiki/Transport_Layer_Security).

Paid Redis Cloud Essentials plans and Redis Cloud Pro plans can use TLS to encrypt data communications between applications and Redis databases.

Note:
  TLS is not available for Free Redis Cloud Essentials plans.

## TLS recommendations

TLS is not enabled by default.

Because TLS has an impact on performance, you need to determine whether the security benefits of TLS are worth the performance impact. TLS recommendations depend on the subscription plan and whether clients connect to your database using public or private endpoints.

This table shows TLS recommendations:

Subscription
PublicÂ endpoint
Private endpoint

Redis Cloud Essentials
Enable TLS
N/A

Redis Cloud Pro
Enable TLS
Enable TLS if security outweighs performance impact

## Client authentication

When you enable TLS, you can optionally require client authentication (also known as "mutual authentication"). If enabled, all clients must present a valid client certificate when they connect to the database.

Client authentication is not required by Redis Cloud; however, it is strongly recommended.

## Enable TLS

To enable TLS for a Redis Cloud database:

Select Databases from the [Redis Cloud console](https://cloud.redis.io/) menu and then select your database from the list.

From the database's Configuration screen, select Edit:

[](/docs/latest/images/rc/button-database-edit.png)

In the Security section, use the Transport layer security (TLS) toggle to enable TLS:

[](/docs/latest/images/rc/database-details-configuration-tab-security-tls-toggle.png)

Select the Download server certificate button to download the Redis Cloud certificate bundle `redis_ca.pem`:

[](/docs/latest/images/rc/button-database-config-security-server-ca-download.png)

Decide whether you want to require client authentication:

If you only want clients that present a valid certificate to be able to connect, continue to the next step.

If you do not want to require client authentication, skip to the final step to apply your changes.

To require client authentication, select the Mutual TLS (require client authentication) checkbox.

Select Add client certificate to add a certificate.

[](/docs/latest/images/rc/mtls-add-client-certificate.png)

Either provide an [X.509 client certificate](https://en.wikipedia.org/wiki/X.509) or chain in PEM format for your client or select Generate to create one:

[](/docs/latest/images/rc/database-details-configuration-tab-security-tls-client-auth-certificate.png)

If you generate your certificate from the Redis Cloud console, a Download certificate button will appear after it is generated. Select it to download the certificate.

[](/docs/latest/images/rc/mtls-download-certificate.png)

The download contains:

`redis-db-<database_id>.crt` â the certificate's public key.

`redis-db-<database_id>.key` â the certificate's private key.

Note:
  You must download the certificate using the button at this point.  After your changes have been applied, the full bundle of public and private keys will no longer be available for download.

If you provide a client certificate or a certificate chain, you will see the certificate details before you save your changes.

[](/docs/latest/images/rc/mtls-certificate-details.png)

Select Save to save the client certificate.

You can select Add client certificate again to add another certificate.

[](/docs/latest/images/rc/mtls-add-client-certificate.png)

To apply your changes and enable TLS, select the Save database button:

[](/docs/latest/images/rc/button-database-save.png)

Note:

When you enable or turn off TLS, the change applies to new connections but does not affect existing connections. Clients must close existing connections and reconnect to apply the change.

Once you've enabled TLS, all client connections to your database must use TLS. Unencrypted connections will no longer be permitted.

## Connect over TLS

To connect to a Redis Cloud database over TLS, you need:

A Redis client that supports TLS
Redis Cloud CA certificates

### Download CA certificates

If you don't have the Redis Cloud CA certificates, you can download them from the Redis Cloud console:

Either select Account Settings from the Redis Cloud console menu or go to the database's Configuration screen.

Go to the Security section.

For Redis Cloud certificate authority, either:

Select the Download button to download the certificates from Redis Cloud certificate authority in Account Settings:

[](/docs/latest/images/rc/button-account-settings-security-ca-download.png)

Select the Download server certificate button to download the certificates from the database's Configuration screen:

[](/docs/latest/images/rc/button-database-config-security-server-ca-download.png)

The download contains a file called `redis_ca.pem`, which includes the following certificates:

Self-signed Redis Cloud Essentials plan Root CA (deprecated but still in use)

Self-signed Redis Cloud Pro plan Root CA and intermediate CA (deprecated but still in use)

Publicly trusted GlobalSign Root CA

Note:
  The downloaded PEM file contains multiple certificates. Make sure to import all certificates to your client trust store. If your client code is not implemented properly, it may only import the first certificate.

To inspect the certificates in `redis_ca.pem`, run the `keytool` command:

```sh
keytool -printcert -file ./redis_ca.pem | grep "Owner:"
```

You can add `redis_ca.pem` to the trust store or pass it directly to a Redis client.

If your database requires client authentication, you also need the public (`redis_user.crt`) and private (`redis_user_private.key`) client keys. See
[Enable TLS](#enable-tls) for details.

### Connect with the Redis CLI

Here's how to use the [Redis CLI](/docs/latest/operate/rs/references/cli-utilities/redis-cli/) to connect to a TLS-enabled Redis Cloud database.

Endpoint and port details are available from the Databases list or the database's Configuration screen.

#### Without client authentication

If your database doesn't require client authentication, then provide the Redis Cloud CA certificate bundle (`redis_ca.pem`) when you connect:

```sh
redis-cli -h <endpoint> -p <port> --tls --cacert redis_ca.pem
```

#### With client authentication

If your database requires client authentication, then you also need to provide your client's private and public keys:

```sh
redis-cli -h <endpoint> -p <port> --tls --cacert redis_ca.pem \
    --cert redis_user.crt --key redis_user_private.key
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page
---
title: "SAML single sign-on"
source: "https://redis.io/docs/latest/operate/rs/security/access-control/saml-sso/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/security/access-control/saml-sso"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "SAML single sign-on"
---
# SAML single sign-on

Set up single sign-on with SAML for the Redis Software Cluster Manager UI.

            Redis Software
          

Redis Software supports both [IdP-initiated](#idp-initiated-sso) and [SP-initiated](#sp-initiated-sso) [single sign-on (SSO)](https://en.wikipedia.org/wiki/Single_sign-on) with [SAML (Security Assertion Markup Language)](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language) for the Cluster Manager UI. Redis Software uses SAML 2.0, which is the latest SAML version and an industry standard.

You cannot use [SCIM (System for Cross-domain Identity Management)](https://en.wikipedia.org/wiki/System_for_Cross-domain_Identity_Management) to provision Redis Software users. However, Redis Software supports just-in-time (JIT) user provisioning, which means Redis Software automatically creates a user account the first time a new user signs in with SSO.

## SSO overview

When single sign-on is activated, users can sign in to the Redis Software Cluster Manager UI using their [identity provider (IdP)](https://en.wikipedia.org/wiki/Identity_provider) instead of usernames and passwords. If [SSO is enforced](#enforce-sso), non-admin users can no longer sign in with their previous usernames and passwords and must use SSO instead.

Before users can sign in to the Cluster Manager UI with SSO, the identity provider admin needs to set up these users on the IdP side with matching email addresses.

With just-in-time (JIT) user provisioning, Redis Software automatically creates user accounts for new users assigned to the SAML application in your identity provider when they sign in to the Cluster Manager UI for the first time. For these users, you must configure the `redisRoleMapping` attribute in your identity provider to assign appropriate roles for [[operate/rs/security/access-control|role-based access control]] during account creation.

### IdP-initiated SSO

With IdP-initiated single sign-on, you can select the Redis Software application after you sign in to your [identity provider (IdP)](https://en.wikipedia.org/wiki/Identity_provider). This redirects you to the Redis Software Cluster Manager UI and signs you in.

### SP-initiated SSO

You can also initiate single sign-on from the Redis Software Cluster Manager UI. This process is known as [service provider (SP)](https://en.wikipedia.org/wiki/Service_provider)-initiated single sign-on.

On the Redis Software Cluster Manager UI's sign-in screen, click Sign in with SSO.

If you already have an active SSO session with your identity provider, this signs you in.

Otherwise, the SSO flow redirects you to your identity provider's sign in screen. Enter your IdP user credentials to sign in. This redirects you back to the Redis Software Cluster Manager UI and automatically signs you in.

Authentication requests expire after 3 minutes.

## IdP requirements

You can use any identity provider to integrate with Redis Software as long as it supports the following:

[SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language) 2.0 protocol.

Signed SAML responses since Redis Software will not accept any unsigned SAML responses.

HTTP-Redirect binding for SP-initiated SSO.

HTTP-POST binding for SAML assertions.

## Set up SAML SSO

To set up SAML single sign-on for a Redis Software cluster:

[Upload the service provider certificate and private key](#upload-sp-certificate).

[Download the service provider metadata](#download-sp-metadata).

[Set up a SAML app](#set-up-app) to integrate Redis Software with your identity provider.

[Download identity provider metadata](#download-idp-metadata).

[Configure SAML identity provider in Redis Software](#configure-idp-metadata).

[Assign the SAML app to existing users](#assign-saml-app-to-existing-users).

[Activate SSO](#activate-sso).

### Upload SP certificate

Create a service provider certificate for Redis Software. See [Create certificates](/docs/latest/operate/rs/security/certificates/create-certificates/#create-certificates) for instructions.

Upload the service provider certificate and key to the Redis Software cluster:

        Cluster Manager UI
      

        REST API
      

Sign in to the Redis Software Cluster Manager UI using admin credentials.

Go to Access Control > Single Sign-On.

In the Service Provider (Redis) metadata section, find Service-provider's public certificate + private key and click Upload.

Enter or upload the private key and certificate for your service provider.

Click Upload to save.

To upload a certificate using the REST API, use an [update cluster certificates](/docs/latest/operate/rs/references/rest-api/requests/cluster/certificates/#put-cluster-certificates) request.

```sh
PUT https://<host>:<port>/v1/cluster/certificates
{
  "certificates": [
    {
      "name": "<cert_name>",
      "certificate": "sso_service",
      "key": "<key>"
    }
  ]
}
```

### Download SP metadata

You need to download the service provider metadata for Redis Software and use it to configure the SAML integration app for your identity provider.

        Cluster Manager UI
      

        REST API
      

To download the service provider's metadata using the Cluster Manager UI:

Go to Access Control > Single Sign-On.

In the Service Provider (Redis) metadata section, click the following buttons to download the service provider files needed to set up a SAML app:

Public certificate

Metadata file

Optionally copy the following values for future SAML app setup in the identity provider. You can also find these values in the service provider's metadata file.

SP entity ID: `https://<cluster-FQDN>/sp`

Assertion Consumer Service (ACS): `https://<cluster-FQDN>:8443/cluster/sso/saml/acs`

Single Logout Service: `https://<cluster-FQDN>:8443/cluster/sso/saml/slo`

To download the service provider's metadata using the REST API, use a [get SAML service provider metadata](/docs/latest/operate/rs/references/rest-api/requests/cluster/sso/#get-cluster-sso-saml-metadata) request.

```sh
GET https://<host>:<port>/v1/cluster/sso/saml/metadata/sp
```

Here's an abridged example of the service provider metadata XML:

```xml
<md:EntityDescriptor xmlns:md="urn:oasis:names:tc:SAML:2.0:metadata" validUntil="2025-12-25T20:38:29" cacheDuration="PT2589134S" entityID="https://<cluster-FQDN>:8443/sp" ID="<ID>">
    ...
    <md:SPSSODescriptor AuthnRequestsSigned="true" WantAssertionsSigned="true" protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
        ...
        <md:SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://<cluster-FQDN>:8443/cluster/sso/saml/slo"/>
        <md:NameIDFormat>urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress</md:NameIDFormat>
        <md:AssertionConsumerService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location="https://<cluster-FQDN>:8443/cluster/sso/saml/acs" index="1"/>
        <md:AttributeConsumingService index="1">
            <md:ServiceName xml:lang="en">Redis Cluster Enterprise - <cluster-FQDN></md:ServiceName>
            <md:ServiceDescription xml:lang="en">Redis Cluster Enterprise SSO</md:ServiceDescription>
            <md:RequestedAttribute Name="firstName" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic" FriendlyName="firstName" isRequired="true"/>
            <md:RequestedAttribute Name="lastName" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic" FriendlyName="lastName" isRequired="true"/>
            <md:RequestedAttribute Name="email" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic" FriendlyName="email" isRequired="true"/>
            <md:RequestedAttribute Name="redisRoleMapping" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic" FriendlyName="redisRoleMapping"/>
        </md:AttributeConsumingService>
    </md:SPSSODescriptor>
</md:EntityDescriptor>
```

See [Metadata for the OASIS Security
Assertion Markup Language (SAML)
V2.0](https://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf) for more information about the metadata fields.

Note:
  Redis Software metadata expiration time is equivalent to the SSO service certificate's expiration time. The service provider metadata will only change if the service address used for the Assertion Consumer Service (ACS) and the single logout (SLO) URL is modified.

### Set up SAML app

Set up a SAML app to integrate Redis Software with your identity provider:

Sign in to your identity provider's admin console.

Create or add a SAML integration app for the service provider Redis Software. For detailed setup instructions, see your identity provider's documentation.

Configure the SAML app with the service provider metadata.

Some identity providers let you upload the XML file directly.

Others require you to manually configure the service provider app with specific metadata fields, such as:

Setting
Value
Description

Audience URI (SP Entity ID)
`https://<cluster-FQDN>:8443/sp`
Unique URL that identifies the Redis Software service provider.

Copy the SP entity ID from the Access Control > Single Sign-On page in the Cluster Manager UI or `EntityDescriptor`'s `entityID` in the metadata XML.

Single sign-on URL
`https://<cluster-FQDN>:8443/cluster/sso/saml/acs`
The service provider endpoint where the identity provider sends a SAML assertion that authenticates a user.

Copy the Assertion Consumer Service (ACS) from the Access Control > Single Sign-On page in the Cluster Manager UI or `AssertionConsumerService`'s `Location` in the metadata XML.

Name ID format
EmailAddress

Application username
Email

For the signature certificate, upload the Service Provider (Redis) public certificate.

Enable signed requests.

Optionally, you can enable single log-out (SLO) to allow users to automatically sign out of the the identity provider when they sign out of the Redis Software Cluster Manager UI. Copy the Single Logout Service from the Access Control > Single Sign-On page in the Cluster Manager UI (`https://<cluster-FQDN>:8443/cluster/sso/saml/slo`) and configure it in the SAML app.

Note:
  Redis Software only supports SP-initiated logout, where the user logs out from the Redis Software Cluster Manager UI. IdP-initiated logout requests are not supported.

Set up your SAML service provider app so the SAML assertion contains the following attributes:

Attribute name (case-sensitive)
Description

firstName
User's first name

lastName
User's last name

email
User's email address (used as the username in the Redis Software Cluster Manager UI and must contain ASCII characters only.)

redisRoleMapping
String array that includes the role UID for role-based access control in Redis Software. Only used for just-in-time (JIT) user provisioning. If a user already exists in Redis Software, this attribute is ignored and their existing roles are preserved.

Note:
  To confirm the identity provider's SAML assertions contain the required attributes, you can use a SAML-tracer web developer tool to inspect them.

Set up any additional configuration required by your identity provider to ensure you can configure the `redisRoleMapping` attribute for SAML users.

If your identity provider lets you configure custom attributes with workflows or group rules, you can set up automation to configure the `redisRoleMapping` field automatically instead of manually.

### Download IdP metadata

After you create the SAML app in your identity provider, retrieve the following information:

Setting
Description

Issuer (IdP entity ID)
The unique entity ID for the identity provider

IdP server URL
The identity provider's HTTPS URL for SAML SSO

Single logout URL
The URL used to sign out of the identity provider and connected apps (optional)

Assertion signing certificate
Public SHA-256 certificate used to validate SAML assertions from the identity provider

You will use this certificate and metadata to configure the identity provider metadata in Redis Software. To find these metadata values, see your identity provider's documentation.

### Configure IdP metadata in Redis Software

After you set up the SAML integration app, you need to configure the identity provider metadata in your Redis Software cluster.

        Cluster Manager UI
      

        REST API
      

Sign in to the Redis Software Cluster Manager UI using admin credentials.

Go to Access Control > Single Sign-On.

In the Identity Provider metadata section, click Edit.

Enter the Identity Provider metadata settings.

Click Save.

Upload your SAML app's assertion signing certificate using an [update cluster certificates](/docs/latest/operate/rs/references/rest-api/requests/cluster/certificates/#put-cluster-certificates) REST API request.

```sh
PUT https://<host>:<port>/v1/cluster/certificates
{
  "certificates": [
    {
      "name": "<cert_name>",
      "certificate": "sso_issuer",
      "key": "<key>"
    }
  ]
}
```

Configure the identity provider metadata using an [update SSO configuration](/docs/latest/operate/rs/references/rest-api/requests/cluster/sso/#put-cluster-sso) REST API request.

```sh
PUT https://<host>:<port>/v1/cluster/sso
{
  "protocol": "saml2",
  "issuer": {
    "id": "urn:sso:example:idp",
    "login_url": "https://idp.example.com/sso/saml",
    "logout_url": "https://idp.example.com/sso/slo"
  }
}
```

### Assign SAML app to existing users

In the identity provider's admin console:

Create user profiles in the identity provider for existing Redis Software users. Make sure each user's email address matches in the identity provider and Redis Software.

Note:
  You do not need to configure the `redisRoleMapping` attribute for existing Redis Software users. Their current roles will be preserved, and the `redisRoleMapping` attribute is ignored if provided.

Assign the new SAML integration app to each user.

See your identity provider's documentation for detailed instructions.

### Activate SSO

After you finish the required SAML SSO configuration between your identity provider and Redis Software cluster, you can activate SSO.

        Cluster Manager UI
      

        REST API
      

To activate single sign-on using the Cluster Manager UI:

Go to Access Control > Single Sign-On.

Click Activate SSO.

To activate single sign-on using the REST API, use an [update SSO configuration](/docs/latest/operate/rs/references/rest-api/requests/cluster/sso/#put-cluster-sso) request.

```sh
PUT https://<host>:<port>/v1/cluster/sso
{
    "control_plane": true
}
```

## Add new users with JIT provisioning

After single sign-on is activated for Redis Software, you can create new Redis Software users on the identity provider side using just-in-time (JIT) provisioning.

In the identity provider's admin console, create a new user profile with a valid email address that contains ASCII characters only. See your identity provider's documentation for detailed instructions.

Configure the `redisRoleMapping` and assign a Redis Software role UID to the user.

Note:

To see a list of available role UIDs in your cluster, use a REST API request to [get all roles](/docs/latest/operate/rs/references/rest-api/requests/roles/#get-all-roles):

```sh
GET https://<host>:<port>/v1/roles
```

Assign the new SAML integration app to the user.

Redis Software will create a new user with the mapped role the first time the new user signs in to the Cluster Manager UI using SSO.

## Enforce SSO

If SSO is enforced for the cluster, non-admin users can no longer sign in with their previous usernames and passwords and must use SSO instead.

        Cluster Manager UI
      

        REST API
      

To enforce single sign-on using the Cluster Manager UI:

Go to Access Control > Single Sign-On.

Find Fallback behavior and click Edit.

Select Enforce SSO-only login.

Click Save.

To enforce single sign-on using the REST API, use an [update SSO configuration](/docs/latest/operate/rs/references/rest-api/requests/cluster/sso/#put-cluster-sso) request.

```sh
PUT https://<host>:<port>/v1/cluster/sso
{
    "enforce_control_plane": true
}
```

## Update configuration

If you change certain metadata or configuration settings after you set up SSO, such as the assertion signing certificate, remember to do the following:

[Update the SAML SSO configuration](#configure-idp-metadata) with the new values.

[Download the updated service provider metadata](#download-sp) and use it to update the Redis Software service provider app.

### Change SP address

If your deployment's default service provider address is not accessible to external identity providers, you can change it to an external hostname.

Warning:
  If you change the service address, the existing SSO integration will break because the metadata file, SP login and logout URLs, and entity ID will change to match the new address. You must update the service provider configuration on the identity provider's side after this change.

To change the service provider address, use an [update SSO configuration](/docs/latest/operate/rs/references/rest-api/requests/cluster/sso/#put-cluster-sso) REST API request:

```sh
PUT https://<host>:<port>/v1/cluster/sso
{
  "service": {
    "address": "https://<external-domain>"
  }
}
```

## Deactivate SSO

        Cluster Manager UI
      

        REST API
      

To deactivate single sign-on using the Cluster Manager UI:

Go to Access Control > Single Sign-On.

Click Deactivate SSO.

Click Confirm.

To deactivate single sign-on using the REST API, use an [update SSO configuration](/docs/latest/operate/rs/references/rest-api/requests/cluster/sso/#put-cluster-sso) request.

```sh
PUT https://<host>:<port>/v1/cluster/sso
{
    "control_plane": false
}
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page
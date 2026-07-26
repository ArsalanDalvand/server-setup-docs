---
title: "Module ngx_http_oidc_module"
source: "https://nginx.org/en/docs/http/ngx_http_oidc_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/ngx_http_oidc_module"
category: "http"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_http_oidc_module"
---
## Module ngx_http_oidc_module

[Example Configuration](#example)
[Directives](#directives)
     [oidc_provider](#oidc_provider)
     [auth_oidc](#auth_oidc)
     [issuer](#issuer)
     [client_id](#client_id)
     [client_secret](#client_secret)
     [config_url](#config_url)
     [cookie_name](#cookie_name)
     [extra_auth_args](#extra_auth_args)
     [frontchannel_logout_uri](#frontchannel_logout_uri)
     [pkce](#pkce)
     [redirect_uri](#redirect_uri)
     [logout_uri](#logout_uri)
     [post_logout_uri](#post_logout_uri)
     [logout_token_hint](#logout_token_hint)
     [scope](#scope)
     [session_store](#session_store)
     [session_timeout](#session_timeout)
     [ssl_crl](#ssl_crl)
     [ssl_trusted_certificate](#ssl_trusted_certificate)
     [userinfo](#userinfo)
[Embedded Variables](#variables)

The `ngx_http_oidc_module` module (1.27.4)
implements authentication as a Relying Party in OpenID Connect using the
[Authorization Code Flow](https://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth).

The module expects the OpenID Provider's configuration to be available via
[metadata](https://openid.net/specs/openid-connect-discovery-1_0.html#ProviderConfig) and requires dynamic
[[http/ngx_http_core_module.html#resolver|resolver]].

The module can be combined with other access modules
via the [[http/ngx_http_core_module.html#satisfy|satisfy]] directive.
Note that the module may still block requests even with
`satisfy any;`
as an OpenID Provider might not redirect the user back to nginx.

 
This module is available as part of our
[commercial subscription](https://www.f5.com/products/nginx).

#### Example Configuration

 

```nginx
http {
    resolver 10.0.0.1;

    oidc_provider my_idp {
        issuer        "https://provider.domain";
        client_id     "unique_id";
        client_secret "unique_secret";
    }

    server {
        location / {
            auth_oidc my_idp;

            proxy_set_header username $oidc_claim_sub;
            proxy_pass       http://backend;
        }
    }
}
```

The example assumes that the
“`https://<nginx-host>/oidc_callback`”
Redirection URI is configured on the OpenID Provider's side.
The path can be customized with the [redirect_uri](#redirect_uri) directive.

#### Directives

            Syntax:
                

`oidc_provider name { ... }`

            Default:
                

            
            —
        
                

            Context:
                

`http`

Defines an OpenID Provider for use with the [auth_oidc](#auth_oidc) directive.

            Syntax:
                

`auth_oidc name | off;`

            Default:
                

```nginx
auth_oidc off;
```

            Context:
                

`http`, `server`, `location`

Enables end user authentication with the
[specified](#oidc_provider) OpenID Provider.

Parameter value can contain variables (1.29.0).

The special value `off` cancels the effect
of the `auth_oidc` directive
inherited from the previous configuration level.

            Syntax:
                

`issuer URL;`

            Default:
                

            
            —
        
                

            Context:
                

`oidc_provider`

Sets the Issuer Identifier URL of the OpenID Provider;
required directive.
The URL must exactly match the value of “`issuer`”
in the OpenID Provider metadata
and requires the “`https`” scheme.

            Syntax:
                

`client_id string;`

            Default:
                

            
            —
        
                

            Context:
                

`oidc_provider`

Specifies the client ID of the Relying Party;
required directive.

            Syntax:
                

`client_secret string;`

            Default:
                

            
            —
        
                

            Context:
                

`oidc_provider`

Specifies a secret value
used to authenticate the Relying Party with the OpenID Provider.
The supported
[authentication
methods](https://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) are
`client_secret_basic` and
`client_secret_post` (1.29.3).
The method is selected based on the OpenID Provider metadata
with a preference to `client_secret_basic`.

            Syntax:
                

`config_url URL;`

            Default:
                

```nginx
config_url <issuer>/.well-known/openid-configuration;
```

            Context:
                

`oidc_provider`

Sets a custom URL to retrieve the OpenID Provider metadata.

            Syntax:
                

`cookie_name name;`

            Default:
                

```nginx
cookie_name NGX_OIDC_SESSION;
```

            Context:
                

`oidc_provider`

Sets the name of a session cookie.

            Syntax:
                

`extra_auth_args string;`

            Default:
                

            
            —
        
                

            Context:
                

`oidc_provider`

Sets additional query arguments for the
[authentication
request](https://openid.net/specs/openid-connect-core-1_0.html#AuthRequest) URL.

 

```nginx
extra_auth_args "display=page&prompt=login";
```

            Syntax:
                

`frontchannel_logout_uri uri;`

            Default:
                

            
            —
        
                

            Context:
                

`oidc_provider`

This directive appeared in version 1.29.3.

Defines the URI path for triggering
[front-channel
logout](https://openid.net/specs/openid-connect-frontchannel-1_0.html).
For the logout request to be associated with a user session,
it must either include the module session cookie or provide
both the “`iss`” and “`sid`” arguments.
It is recommended to configure the OpenID Provider to set the
“`iss`” and “`sid`” arguments
when invoking this endpoint.

            Syntax:
                

`pkce on | off;`

            Default:
                

            
            —
        
                

            Context:
                

`oidc_provider`

This directive appeared in version 1.29.3.

Explicitly enables or disables PKCE.
By default, PKCE is automatically enabled
based on OpenID Provider metadata.

            Syntax:
                

`redirect_uri uri;`

            Default:
                

```nginx
redirect_uri /oidc_callback;
```

            Context:
                

`oidc_provider`

Defines the Redirection URI path for post-authentication redirects
expected by the module from the OpenID Provider.
The `uri` must match the configuration on the Provider's side.

Absolute “`https`” URIs are supported since 1.29.0.

            Syntax:
                

`logout_uri uri;`

            Default:
                

            
            —
        
                

            Context:
                

`oidc_provider`

This directive appeared in version 1.29.0.

Defines the URI path for initiating session logout.
Upon session termination, the user is redirected to
[Provider's
Logout Endpoint](https://openid.net/specs/openid-connect-rpinitiated-1_0.html#OPMetadata)
or to the [post logout page](#post_logout_uri).
If neither is configured, the built-in post logout page is displayed.

            Syntax:
                

`post_logout_uri uri;`

            Default:
                

            
            —
        
                

            Context:
                

`oidc_provider`

This directive appeared in version 1.29.0.

Defines the path or absolute URI
to redirect the user to after the logout.
The `uri` must match the configuration on the Provider's side.
If the post logout page is served by NGINX,
the OIDC module shouldn't be enabled for this location:

 

```nginx
http {
    oidc_provider my_idp {
        ...

        logout_uri      /logout;
        post_logout_uri /logged_out_page.html;
    }

    server {
        auth_oidc my_idp;

        location /logged_out_page.html {
            auth_oidc off;
        }
    }
}
```

            Syntax:
                

`logout_token_hint on | off;`

            Default:
                

```nginx
logout_token_hint off;
```

            Context:
                

`oidc_provider`

This directive appeared in version 1.29.0.

Adds the
[`id_token_hint`](https://openid.net/specs/openid-connect-rpinitiated-1_0.html#RPLogout)
argument to the
[Provider's
Logout Endpoint](https://openid.net/specs/openid-connect-rpinitiated-1_0.html#OPMetadata)
when redirecting user during logout.
This argument can be required by some OpenID Providers.

            Syntax:
                

`scope scope ...;`

            Default:
                

```nginx
scope openid;
```

            Context:
                

`oidc_provider`

Sets requested scopes.
The `openid` scope is always required by OIDC.

            Syntax:
                

`session_store name;`

            Default:
                

            
            —
        
                

            Context:
                

`oidc_provider`

Specifies a custom
[[http/ngx_http_keyval_module.html#keyval_zone|key-value database]]
that stores session data.
By default, an 8-megabyte key-value database named 
`oidc_default_store_<provider name>`
is created automatically.

 
A separate key-value database should be configured for each Provider
to prevent session reuse across providers.

            Syntax:
                

`session_timeout time;`

            Default:
                

```nginx
session_timeout 8h;
```

            Context:
                

`oidc_provider`

Sets a timeout after which the session is deleted, unless it was
[refreshed](https://openid.net/specs/openid-connect-core-1_0.html#RefreshTokens).

            Syntax:
                

`ssl_crl file;`

            Default:
                

            
            —
        
                

            Context:
                

`oidc_provider`

Specifies a `file` with revoked certificates (CRL)
in the PEM format used to verify
the certificates of the OpenID Provider endpoints.
When using intermediate certificates, their CRLs should be
specified in the same file.

            Syntax:
                

`ssl_trusted_certificate file;`

            Default:
                

```nginx
ssl_trusted_certificate system CA bundle;
```

            Context:
                

`oidc_provider`

Specifies a `file` with trusted CA certificates in the PEM format
used to verify
the certificates of the OpenID Provider endpoints.

            Syntax:
                

`userinfo on | off;`

            Default:
                

```nginx
userinfo off;
```

            Context:
                

`oidc_provider`

This directive appeared in version 1.29.0.

Enables downloading of the
[UserInfo](https://openid.net/specs/openid-connect-core-1_0.html#UserInfo)
data and makes UserInfo claims available via the 
[$oidc_claim_`name`](#var_oidc_claim_) variables.

#### Embedded Variables

The `ngx_http_oidc_module` module supports embedded variables:

 
`$oidc_id_token`

ID token

`$oidc_access_token`

access token

`$oidc_claim_``name`

top-level ID token or UserInfo claim

Nested claims can be fetched with the
[[http/ngx_http_auth_jwt_module.html|auth_jwt]] module:

 

```nginx
http {
    auth_jwt_claim_set $postal_code address postal_code;

    server {
        location / {
            auth_oidc my_idp;
            auth_jwt  off token=$oidc_id_token;

            proxy_set_header x-postal_code $postal_code;
            proxy_pass       http://backend;
        }
    }
}
```

`$oidc_userinfo`

UserInfo data in the JSON format (1.29.0)
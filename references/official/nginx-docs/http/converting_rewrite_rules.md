---
title: "Converting rewrite rules"
source: "https://nginx.org/en/docs/http/converting_rewrite_rules.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/converting_rewrite_rules"
category: "http"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Converting rewrite rules"
---
## Converting rewrite rules

[Converting Mongrel rules](#converting_mongrel_rules)

#### A redirect to a main site

People who during their shared hosting life used to configure
everything using only Apache’s .htaccess files,
usually translate the following rules:

 

```nginx
RewriteCond  %{HTTP_HOST}  example.org
RewriteRule  (.*)          http://www.example.org$1
```

to something like this:

 

```nginx
server {
    listen       80;
    server_name  www.example.org  example.org;
    if ($http_host = example.org) {
        rewrite  (.*)  http://www.example.org$1;
    }
    ...
}
```

This is a wrong, cumbersome, and ineffective way.
The right way is to define a separate server for `example.org`:

 

```nginx
server {
    listen       80;
    server_name  example.org;
    return       301 http://www.example.org$request_uri;
}

server {
    listen       80;
    server_name  www.example.org;
    ...
}
```

 
On versions prior to 0.9.1, redirects can be made with:

```nginx
rewrite      ^ http://www.example.org$request_uri?;
```

Another example.
Instead of the “upside-down” logic “all that is not
`example.com` and is not `www.example.com`”:

 

```nginx
RewriteCond  %{HTTP_HOST}  !example.com
RewriteCond  %{HTTP_HOST}  !www.example.com
RewriteRule  (.*)          http://www.example.com$1
```

one should simply define `example.com`, `www.example.com`,
and “everything else”:

 

```nginx
server {
    listen       80;
    server_name  example.com www.example.com;
    ...
}

server {
    listen       80 default_server;
    server_name  _;
    return       301 http://example.com$request_uri;
}
```

 
On versions prior to 0.9.1, redirects can be made with:

```nginx
rewrite      ^ http://example.com$request_uri?;
```

#### Converting Mongrel rules

Typical Mongrel rules:

 

```nginx
DocumentRoot /var/www/myapp.com/current/public

RewriteCond %{DOCUMENT_ROOT}/system/maintenance.html -f
RewriteCond %{SCRIPT_FILENAME} !maintenance.html
RewriteRule ^.*$ %{DOCUMENT_ROOT}/system/maintenance.html [L]

RewriteCond %{REQUEST_FILENAME} -f
RewriteRule ^(.*)$ $1 [QSA,L]

RewriteCond %{REQUEST_FILENAME}/index.html -f
RewriteRule ^(.*)$ $1/index.html [QSA,L]

RewriteCond %{REQUEST_FILENAME}.html -f
RewriteRule ^(.*)$ $1.html [QSA,L]

RewriteRule ^/(.*)$ balancer://mongrel_cluster%{REQUEST_URI} [P,QSA,L]
```

should be converted to

 

```nginx
location / {
    root       /var/www/myapp.com/current/public;

    try_files  /system/maintenance.html
               $uri  $uri/index.html $uri.html
               @mongrel;
}

location @mongrel {
    proxy_pass  http://mongrel;
}
```
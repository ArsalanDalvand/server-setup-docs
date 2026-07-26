---
title: "nginx documentation"
source: "https://nginx.org/en/docs/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "en/docs"
category: "en"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "nginx documentation"
---
## nginx documentation

#### Introduction

 

[[install.html|Installing nginx]]

[[configure.html|Building nginx from Sources]]

[[beginners_guide.html|Beginner’s Guide]]

[Admin’s Guide](https://docs.nginx.com/nginx/admin-guide/)

[[control.html|Controlling nginx]]

[[events.html|Connection processing methods]]

[[hash.html|Setting up hashes]]

[[debugging_log.html|A debugging log]]

[[syslog.html|Logging to syslog]]

[[syntax.html|Configuration file measurement units]]

[[switches.html|Command-line parameters]]

[[windows.html|nginx for Windows]]

[[quic.html|Support for QUIC and HTTP/3]]

 

[[http/request_processing.html|How nginx processes a request]]

[[http/server_names.html|Server names]]

[[http/load_balancing.html|Using nginx as HTTP load balancer]]

[[http/configuring_https_servers.html|Configuring HTTPS servers]]

 

[[stream/stream_processing.html|How nginx processes a TCP/UDP session]]

 

[Scripting with njs](njs/index.html)

 

[Chapter “nginx” in
“The Architecture of Open Source Applications”](http://www.aosabook.org/en/nginx.html)

#### How-To

 

[[howto_build_on_win32.html|Building nginx on the Win32 platform with Visual C]]

[Installing NGINX Plus AMIs on Amazon EC2](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-plus-amazon-web-services/)

[[nginx_dtrace_pid_provider.html|Debugging nginx with DTrace pid provider]]

 

[[http/converting_rewrite_rules.html|Converting rewrite rules]]

[[http/websocket.html|WebSocket proxying]]

#### Development

 

[[contributing_changes.html|Contributing Changes]]

[[dev/development_guide.html|Development guide]]

#### Modules reference

 

[Alphabetical index of directives](dirindex.html)

[Alphabetical index of variables](varindex.html)

 

[[ngx_core_module.html|Core functionality]]

 

[[http/ngx_http_core_module.html|ngx_http_core_module]]

[[http/ngx_http_access_module.html|ngx_http_access_module]]

[[http/ngx_http_addition_module.html|ngx_http_addition_module]]

[[http/ngx_http_api_module.html|ngx_http_api_module]]

[[http/ngx_http_auth_basic_module.html|ngx_http_auth_basic_module]]

[[http/ngx_http_auth_jwt_module.html|ngx_http_auth_jwt_module]]

[[http/ngx_http_auth_request_module.html|ngx_http_auth_request_module]]

[[http/ngx_http_auth_require_module.html|ngx_http_auth_require_module]]

[[http/ngx_http_autoindex_module.html|ngx_http_autoindex_module]]

[[http/ngx_http_browser_module.html|ngx_http_browser_module]]

[[http/ngx_http_charset_module.html|ngx_http_charset_module]]

[[http/ngx_http_dav_module.html|ngx_http_dav_module]]

[[http/ngx_http_empty_gif_module.html|ngx_http_empty_gif_module]]

[[http/ngx_http_f4f_module.html|ngx_http_f4f_module]]

[[http/ngx_http_fastcgi_module.html|ngx_http_fastcgi_module]]

[[http/ngx_http_flv_module.html|ngx_http_flv_module]]

[[http/ngx_http_geo_module.html|ngx_http_geo_module]]

[[http/ngx_http_geoip_module.html|ngx_http_geoip_module]]

[[http/ngx_http_grpc_module.html|ngx_http_grpc_module]]

[[http/ngx_http_gunzip_module.html|ngx_http_gunzip_module]]

[[http/ngx_http_gzip_module.html|ngx_http_gzip_module]]

[[http/ngx_http_gzip_static_module.html|ngx_http_gzip_static_module]]

[[http/ngx_http_headers_module.html|ngx_http_headers_module]]

[[http/ngx_http_hls_module.html|ngx_http_hls_module]]

[[http/ngx_http_image_filter_module.html|ngx_http_image_filter_module]]

[[http/ngx_http_index_module.html|ngx_http_index_module]]

[[http/ngx_http_internal_redirect_module.html|ngx_http_internal_redirect_module]]

[[http/ngx_http_js_module.html|ngx_http_js_module]]

[[http/ngx_http_keyval_module.html|ngx_http_keyval_module]]

[[http/ngx_http_limit_conn_module.html|ngx_http_limit_conn_module]]

[[http/ngx_http_limit_req_module.html|ngx_http_limit_req_module]]

[[http/ngx_http_log_module.html|ngx_http_log_module]]

[[http/ngx_http_map_module.html|ngx_http_map_module]]

[[http/ngx_http_memcached_module.html|ngx_http_memcached_module]]

[[http/ngx_http_mirror_module.html|ngx_http_mirror_module]]

[[http/ngx_http_mp4_module.html|ngx_http_mp4_module]]

[[http/ngx_http_num_map_module.html|ngx_http_num_map_module]]

[[http/ngx_http_oidc_module.html|ngx_http_oidc_module]]

[[http/ngx_http_perl_module.html|ngx_http_perl_module]]

[[http/ngx_http_proxy_module.html|ngx_http_proxy_module]]

[[http/ngx_http_proxy_protocol_vendor_module.html|ngx_http_proxy_protocol_vendor_module]]

[[http/ngx_http_random_index_module.html|ngx_http_random_index_module]]

[[http/ngx_http_realip_module.html|ngx_http_realip_module]]

[[http/ngx_http_referer_module.html|ngx_http_referer_module]]

[[http/ngx_http_rewrite_module.html|ngx_http_rewrite_module]]

[[http/ngx_http_scgi_module.html|ngx_http_scgi_module]]

[[http/ngx_http_secure_link_module.html|ngx_http_secure_link_module]]

[[http/ngx_http_session_log_module.html|ngx_http_session_log_module]]

[[http/ngx_http_slice_module.html|ngx_http_slice_module]]

[[http/ngx_http_split_clients_module.html|ngx_http_split_clients_module]]

[[http/ngx_http_ssi_module.html|ngx_http_ssi_module]]

[[http/ngx_http_ssl_module.html|ngx_http_ssl_module]]

[[http/ngx_http_status_module.html|ngx_http_status_module]]

[[http/ngx_http_stub_status_module.html|ngx_http_stub_status_module]]

[[http/ngx_http_sub_module.html|ngx_http_sub_module]]

[[http/ngx_http_tunnel_module.html|ngx_http_tunnel_module]]

[[http/ngx_http_upstream_module.html|ngx_http_upstream_module]]

[[http/ngx_http_upstream_conf_module.html|ngx_http_upstream_conf_module]]

[[http/ngx_http_upstream_hc_module.html|ngx_http_upstream_hc_module]]

[[http/ngx_http_userid_module.html|ngx_http_userid_module]]

[[http/ngx_http_uwsgi_module.html|ngx_http_uwsgi_module]]

[[http/ngx_http_v2_module.html|ngx_http_v2_module]]

[[http/ngx_http_v3_module.html|ngx_http_v3_module]]

[[http/ngx_http_xslt_module.html|ngx_http_xslt_module]]

 

[[mail/ngx_mail_core_module.html|ngx_mail_core_module]]

[[mail/ngx_mail_auth_http_module.html|ngx_mail_auth_http_module]]

[[mail/ngx_mail_proxy_module.html|ngx_mail_proxy_module]]

[[mail/ngx_mail_realip_module.html|ngx_mail_realip_module]]

[[mail/ngx_mail_ssl_module.html|ngx_mail_ssl_module]]

[[mail/ngx_mail_imap_module.html|ngx_mail_imap_module]]

[[mail/ngx_mail_pop3_module.html|ngx_mail_pop3_module]]

[[mail/ngx_mail_smtp_module.html|ngx_mail_smtp_module]]

 

[[stream/ngx_stream_core_module.html|ngx_stream_core_module]]

[[stream/ngx_stream_access_module.html|ngx_stream_access_module]]

[[stream/ngx_stream_geo_module.html|ngx_stream_geo_module]]

[[stream/ngx_stream_geoip_module.html|ngx_stream_geoip_module]]

[[stream/ngx_stream_js_module.html|ngx_stream_js_module]]

[[stream/ngx_stream_keyval_module.html|ngx_stream_keyval_module]]

[[stream/ngx_stream_limit_conn_module.html|ngx_stream_limit_conn_module]]

[[stream/ngx_stream_log_module.html|ngx_stream_log_module]]

[[stream/ngx_stream_map_module.html|ngx_stream_map_module]]

[[stream/ngx_stream_mqtt_preread_module.html|ngx_stream_mqtt_preread_module]]

[[stream/ngx_stream_mqtt_filter_module.html|ngx_stream_mqtt_filter_module]]

[[stream/ngx_stream_num_map_module.html|ngx_stream_num_map_module]]

[[stream/ngx_stream_pass_module.html|ngx_stream_pass_module]]

[[stream/ngx_stream_proxy_module.html|ngx_stream_proxy_module]]

[[stream/ngx_stream_proxy_protocol_vendor_module.html|ngx_stream_proxy_protocol_vendor_module]]

[[stream/ngx_stream_realip_module.html|ngx_stream_realip_module]]

[[stream/ngx_stream_return_module.html|ngx_stream_return_module]]

[[stream/ngx_stream_set_module.html|ngx_stream_set_module]]

[[stream/ngx_stream_split_clients_module.html|ngx_stream_split_clients_module]]

[[stream/ngx_stream_ssl_module.html|ngx_stream_ssl_module]]

[[stream/ngx_stream_ssl_preread_module.html|ngx_stream_ssl_preread_module]]

[[stream/ngx_stream_upstream_module.html|ngx_stream_upstream_module]]

[[stream/ngx_stream_upstream_hc_module.html|ngx_stream_upstream_hc_module]]

[[stream/ngx_stream_zone_sync_module.html|ngx_stream_zone_sync_module]]

 

[[ngx_google_perftools_module.html|ngx_google_perftools_module]]

[[ngx_mgmt_module.html|ngx_mgmt_module]]

 

[[http/ngx_http_acme_module.html|ngx_http_acme_module]]

[[ngx_otel_module.html|ngx_otel_module]]
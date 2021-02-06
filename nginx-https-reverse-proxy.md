# nginx Reverse proxy with HTTPS support

* Mozilla SSL Configuration Generator: **intermediate config** (tested on `nginx/1.14.2`)
*  SSL Labs's 'SSL Server Rating Guide': **A+**

## Config
File `/etc/nginx/sites-available/default`:

```sh
# Reverse proxy with HTTPS for 127.0.0.1:5000
server {
    listen 80;
    return 301 https://$host$request_uri;
}

server {

    listen 443 ssl http2;
    server_name localhost:5000;

    ssl_certificate           /etc/ssl/example.com/fullchain1.pem;
    ssl_certificate_key       /etc/ssl/example.com/privkey1.pem;

    ssl on;
    ssl_session_cache  builtin:1000  shared:SSL:10m;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
    ssl_prefer_server_ciphers on;

    # HSTS (ngx_http_headers_module is required) (63072000 seconds)
    add_header Strict-Transport-Security "max-age=63072000" always;

    access_log            /var/log/nginx/access.log;

    location / {

      proxy_set_header        Host $host;
      proxy_set_header        X-Real-IP $remote_addr;
      proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header        X-Forwarded-Proto $scheme;

      # Fix the “It appears that your reverse proxy set up is broken" error.
      proxy_pass          http://localhost:5000;
      proxy_read_timeout  90;

      proxy_redirect      http://localhost:5000 https://example.com;
    }
  }
  ```
  ## Sources
  * https://www.digitalocean.com/community/tutorials/how-to-configure-nginx-with-ssl-as-a-reverse-proxy-for-jenkins
  * https://ssl-config.mozilla.org/#server=nginx&version=1.17.7&config=intermediate&openssl=1.1.1d&guideline=5.6

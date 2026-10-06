# Nginx on Wodby

What Wodby sets up for this web server. Check it before adding an `nginx.conf` to the repository: Nginx here is configured by environment variables and by the config files the service declares, not by files in the codebase.

## How it is configured

On every start the container renders its configuration from templates and environment variables: `/etc/nginx/nginx.conf`, `/etc/nginx/conf.d/vhost.conf` (one server), `/etc/nginx/preset.conf`, `/etc/nginx/upstream.conf`, `/etc/nginx/defaults.conf` and `/etc/nginx/fastcgi.conf`. They are rewritten on each start; never edit them in a running container.

To change the configuration:

- Set `NGINX_*` environment variables on the service.
- Change the `docroot` setting.
- Override a declared config file: the main config template and the virtual host template of the image. Services built on this one also declare their preset template.

## Preset

`NGINX_VHOST_PRESET` selects the location rules included in the server. This service uses `html`: files are served from the document root, a directory request serves the index file (`NGINX_INDEX_FILE`), and a path that matches no file falls back to the index file, which suits a single-page application. The image also ships the presets `php`, `http-proxy`, `wordpress`, `laravel`, `matomo`, `django` and several Drupal versions; services built on this one set the preset for their application.

## Document root

The codebase is at `/var/www/html`. The `docroot` setting (variable `DOCROOT_SUBDIR`) names a subdirectory of the repository, and `NGINX_SERVER_ROOT` is set to `/var/www/html/` followed by it. Leave the setting empty to serve the repository root.

## Backend link

An optional link to another service sets `NGINX_BACKEND_HOST` and `NGINX_BACKEND_PORT`. They form the upstream only for presets that have one: the PHP presets pass PHP requests to it over FastCGI, `http-proxy` and `django` proxy HTTP to it. With the `html` preset the upstream is empty and the link has no effect.

## Always present

Unless `NGINX_VHOST_NO_DEFAULTS` is set, these rules apply with every preset:

- `/.healthz` returns 204. Use it for health checks.
- Paths that start with a dot are denied, except `/.well-known/`. `NGINX_ALLOW_ACCESS_HIDDEN_FILES` lifts the rule. `wodby.yml` and `Makefile` are denied.
- `/favicon.ico` returns an empty image when the file is missing.
- Response headers `X-XSS-Protection`, `X-Frame-Options`, `X-Content-Type-Options` and `Content-Security-Policy` are added. The policy only restricts framing; set `NGINX_HEADERS_CONTENT_SECURITY_POLICY` to change it or `NGINX_NO_DEFAULT_HEADERS` to drop all four.

## Variables that matter most

| Variable | Effect |
| --- | --- |
| `NGINX_CLIENT_MAX_BODY_SIZE` | largest accepted request body, such as an upload |
| `NGINX_STATIC_EXT_REGEX` | file extensions treated as static files |
| `NGINX_STATIC_EXPIRES` | cache lifetime sent with static files |
| `NGINX_DISABLE_CACHING` | sends no-cache headers for everything |
| `NGINX_INDEX_FILE` | index file |
| `NGINX_ERROR_403_URI`, `NGINX_ERROR_404_URI` | custom error pages |
| `NGINX_SERVER_EXTRA_CONF_FILEPATH` | an extra file included in the server block |
| `NGINX_SET_REAL_IP_FROM`, `NGINX_REAL_IP_HEADER` | trusted proxy and the header holding the client address |
| `NGINX_GZIP`, `NGINX_BROTLI` | response compression |

Access and error logs go to the container's standard output and error.

## Build and reaching the service

- The image is built from the connected repository: the codebase is copied into the image at `/var/www/html`. Files are served from the built image, so a change needs a new build and deployment.
- Other services reach it over HTTP on port 80 at the app service's name inside the environment.

## Check the result

- `nginx -T` prints the configuration in effect.
- `curl -sI localhost/.healthz` returns 204 from inside the container.

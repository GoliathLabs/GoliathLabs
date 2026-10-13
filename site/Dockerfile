# The site as an image: Hugo renders it, nginx serves the result as a
# non-root user on port 8080.
FROM ghcr.io/gohugoio/hugo:v0.167.0@sha256:7bb99a126eddeea8fbfaacedc5ef6708805411bdeaa01e668405a137fd02d048 AS build

# The address the site is built for, and whether it asks search engines to
# stay away. The defaults are the real site; the copy on GitHub Pages sets
# both (.github/workflows/site.yml).
ARG HUGO_BASEURL=https://felix-golatofski.de/
ARG HUGO_PARAMS_NOINDEX=false

COPY --chown=hugo:hugo . /project
RUN HUGO_PARAMS_NOINDEX=$HUGO_PARAMS_NOINDEX hugo build --gc --minify --panicOnWarning --baseURL "$HUGO_BASEURL"

# Only the rendered files, for "--target static --output type=local".
FROM scratch AS static
COPY --from=build /project/public /

# The stable branch of nginx: even minor versions.
FROM docker.io/nginxinc/nginx-unprivileged:1.30.5-alpine-slim@sha256:7f79f463c6eb0d0bf8782992bb9636434398e3e5848398805aef2c9b025d8609

COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /project/public /usr/share/nginx/html

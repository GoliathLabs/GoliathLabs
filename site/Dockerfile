# The site as an image: Hugo renders it, nginx serves the result as a
# non-root user on port 8080.
FROM ghcr.io/gohugoio/hugo:v0.169.0@sha256:a146e0639230bbc73358e6563240f0a6103466f0390b80a457c775068ec802a9 AS build

# The address the site is built for. The default is the real site; the
# copy on GitHub Pages sets its own (.github/workflows/site.yml).
ARG HUGO_BASEURL=https://felix-golatofski.de/

COPY --chown=hugo:hugo . /project
RUN hugo build --gc --minify --panicOnWarning --baseURL "$HUGO_BASEURL"

# Only the rendered files, for "--target static --output type=local".
FROM scratch AS static
COPY --from=build /project/public /

# The stable branch of nginx: even minor versions.
FROM docker.io/nginxinc/nginx-unprivileged:1.30.6-alpine-slim@sha256:5d8bb9437d74a600ec5dbf90c035451c724dc33eac8d3a71078f6f8612c345f6

# The base only carries ca-certificates-bundle, which still holds roots Mozilla
# has removed; the ca-certificates package regenerates the store without them.
USER root
RUN apk add --no-cache ca-certificates
USER 101

COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /project/public /usr/share/nginx/html

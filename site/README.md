# felix-golatofski.de

The personal site: a [Hugo](https://gohugo.io) project with its own small layout and no theme.

- `content/_index.md`: the text of the start page.
- `hugo.toml`: title, description and the links below the text (`params.links`).
- `content/<name>.md`: a further page at `/<name>/`.
- `layouts/`, `assets/css/main.css`: markup and style. The page loads no scripts and nothing from other hosts; `nginx.conf` enforces that with a Content-Security-Policy.

## Preview

```sh
docker build -t website site
docker run --rm -p 8080:8080 website
```

Then open <http://localhost:8080>.

## Publishing

A push to `main` that touches `site/` builds `ghcr.io/goliathlabs/website` (`.github/workflows/site.yml`), tagged `latest` and `sha-<commit>`. The cluster pins the image by digest, so a new build goes live once the digest there has been raised.

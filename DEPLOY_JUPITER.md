# JALT on Jupiter One

Hostname: https://jalt.andersoncomputer.net/

Build the static website with Hugo Extended:

```bash
hugo --config config.yaml,config.jupiter.yaml --gc --minify
```

The Jupiter configuration selects the requested hostname and leaves analytics
disabled until the new hostname has been configured with the analytics provider.
Hugo builds the stylesheet through its asset pipeline. The homepage provides RSS
links; contact uses email because no form backend is connected. Draft content is
excluded. Section content and the Content navigation page are included.

Copy only the generated public directory to
`/data/www/jalt.andersoncomputer.net` on Jupiter One. The existing nginx sites
container serves it. A dedicated file-provider route under
`/opt/traefik/dynamic/jalt-subdomain.yml` selects this hostname, the existing
`static` service and the production `le` certificate resolver on `websecure`.
No new host ports, services or DNS changes are needed for the existing CNAME.

The repository's legacy main-branch workflows target GitHub Pages and trixie01.
This Jupiter deployment is manual; the review branch does not trigger those jobs.
Do not merge deployment changes without deciding how those legacy targets should
be maintained. The original justalittletechnical.com content is left in place.

Validation: clean Hugo build and all generated internal page/asset references.
Deployment validation additionally checks uploaded files, nginx syntax, HTTPS,
main pages and browser rendering. No contact form or newsletter service is provisioned.

Rollback: move only the dedicated `jalt-subdomain.yml` route outside Traefik's
watched dynamic directory. Retain the new content directory for recovery.

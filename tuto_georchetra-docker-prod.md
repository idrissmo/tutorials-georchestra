# Tuto : deploy georchetra with docker, on production

Here, we are going to explain how to adapt the docker
composition offrered by the geoochetra community,
 to run it  on production.
 This is problay stillnot really a production-ready deployement,
 but will work on a remote server, with a custom domain name an SSL
 certificat.

 ## step 1 : clone the official repo
```bash
 git clone --recurse-submodules https://github.com/georchestra/docker.git
 cd docker
 git checkout 23.0 && git submodule update

 ```
 ## step 2 : change traefik configuration file
 We will add a CertificateResolver:

*resources/traefic_custom.yaml:*
 ```yaml
 global: 
  sendAnonymousUsage: false
  checkNewVersion: false

entryPoints:
  web:
    address: ":80"
    http:
      redirections:
        entryPoint:
          to: websecure
          scheme: https

  websecure:
    address: ":443"

providers:
  docker:
    watch: true
    exposedByDefault: false
    endpoint: unix:///var/run/docker.sock

api:
  dashboard: true

log:
  level: INFO

certificatesResolvers:
  letsEncrypt:
    acme:
      # comment this line when going to production
      caServer: https://acme-staging-v02.api.letsencrypt.org/directory
      email: jean.pommier@pi-geosolutions.fr
      storage: /acme/acme.json
      httpChallenge:
        entryPoint: web

ping: {}
```

## step 3: upadate according traefik config in docker-compose.override.yml
In docker-compose.override.yml:
- we can comment the `traefic-me-certificate-downloader` block 
(optional but not useful anymore)
- for `georchestra-127-0-1-1.traefic.me` service:
    - comment the `depends_on` section
    - change the `volumes` section for:
```
    -/var/run/docker.stock:/var/run/docker.sock:ro
    -./ressources/traefic_custom.yml:/etc/traefic/traefic.yml:ro
    -acme:/acme
```
- TODO: document changing FQDN and labels 
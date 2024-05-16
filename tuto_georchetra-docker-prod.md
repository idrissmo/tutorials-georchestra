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
 ## step 2 : 
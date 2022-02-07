---
title: 使用Docker搭建Verdaccio服务
date: 2021-06-17 14:10:27
tags: [docker, verdaccio, npm]
---

[Verdaccio][]是一个 Node.js创建的轻量的私有`npm proxy registry`。

[Verdaccio]: https://verdaccio.org/

## Run Server

### Run server with local storage

`docker-compose.yaml`文件内容如下：

```yaml
version: '3.7'

services:
  verdaccio:
    image: verdaccio/verdaccio
    restart: always
    labels:
      - traefik.http.routers.verdaccio.rule=Host(`verdaccio.yourdomain.com`)
      - traefik.http.routers.verdaccio.entrypoints=websecure
      - traefik.http.routers.verdaccio.service=verdaccio
      - traefik.http.services.verdaccio.loadbalancer.server.port=4873
    volumes:
      - storage:/verdaccio/storage

volumes:
  storage:
```

最后，使用下面命令启动服务：

```bash
docker-compose up -d
```

<!--more-->

### Run server with s3 (minio)

{% tabs %}
<!-- tab Dockerfile -->
```Dockerfile
FROM verdaccio/verdaccio

USER root
ENV NODE_ENV=production
RUN npm set registry https://registry.npm.taobao.org/ \
    && npm install -g verdaccio-aws-s3-storage \
    && ln -s /usr/local/lib/node_modules/verdaccio-aws-s3-storage /verdaccio/plugins/verdaccio-aws-s3-storage
COPY config.yaml /verdaccio/conf/config.yaml
USER verdaccio
```
<!-- endtab -->

<!-- tab config.yaml -->
```yaml
#
# This is the config file used for the docker images.
# It allows all users to do anything, so don't use it on production systems.
#
# Do not configure host and port under `listen` in this file
# as it will be ignored when using docker.
# see https://verdaccio.org/docs/en/docker#docker-and-custom-port-configuration
#
# Look here for more config file examples:
# https://github.com/verdaccio/verdaccio/tree/master/conf
#

# path to a directory with all packages
storage: /verdaccio/storage/data
# path to a directory with plugins to include
plugins: /verdaccio/plugins

web:
  # WebUI is enabled as default, if you want disable it, just uncomment this line
  #enable: false
  title: Verdaccio
  # comment out to disable gravatar support
  gravatar: true
  # by default packages are ordercer ascendant (asc|desc)
  # sort_packages: asc
  # darkMode: true
  # logo: http://somedomain/somelogo.png
  # favicon: http://somedomain/favicon.ico | /path/favicon.ico

# translate your registry, api i18n not available yet
# i18n:
# list of the available translations https://github.com/verdaccio/ui/tree/master/i18n/translations
#   web: en-US

auth:
  htpasswd:
    file: /verdaccio/storage/htpasswd
    # Maximum amount of users allowed to register, defaults to "+infinity".
    # You can set this to -1 to disable registration.
    # max_users: 1000

# a list of other known repositories we can talk to
uplinks:
  npmjs:
    url: https://registry.npmjs.org/

packages:
  '@*/*':
    # scoped packages
    access: $all
    publish: $authenticated
    unpublish: $authenticated
    proxy: npmjs

  '**':
    proxy: npmjs

# You can specify HTTP/1.1 server keep alive timeout in seconds for incoming connections.
# A value of 0 makes the http server behave similarly to Node.js versions prior to 8.0.0, which did not have a keep-alive timeout.
# WORKAROUND: Through given configuration you can workaround following issue https://github.com/verdaccio/verdaccio/issues/301. Set to 0 in case 60 is not enough.
server:
  keepAliveTimeout: 60

middlewares:
  audit:
    enabled: true

# log settings
logs: { type: stdout, format: pretty, level: http }

#experiments:
#  # support for npm token command
#  token: false
#  # enable tarball URL redirect for hosting tarball with a different server, the tarball_url_redirect can be a template string
#  tarball_url_redirect: 'https://mycdn.com/verdaccio/${packageName}/${filename}'
#  # the tarball_url_redirect can be a function, takes packageName and filename and returns the url, when working with a js configuration file
#  tarball_url_redirect(packageName, filename) {
#    const signedUrl = // generate a signed url
#    return signedUrl;
#  }

# This affect the web and api (not developed yet)
#i18n:
#web: en-US

store:
  aws-s3-storage:
    bucket: verdaccio
    # keyPrefix: some-prefix # optional, has the effect of nesting all files in a subdirectory
    # region: us-west-2 # optional, will use aws s3's default behavior if not specified
    endpoint: https://minio.yourdomain.com # optional, will use aws s3's default behavior if not specified
    s3ForcePathStyle: true # optional, will use path style URLs for S3 objects
    tarballACL: private # optional, use public-read to work with CDN like Amazon CloudFront
    accessKeyId: pEPnuTGrb7fKm7bdFEcydE85ZXNrzKr4 # optional, aws accessKeyId for private S3 bucket
    secretAccessKey: k5K8RVMpBeRDtiTxEvWk9oFWTySYYwwP # optional, aws secretAccessKey for private S3 bucket
    # sessionToken: your-session-token # optional, aws sessionToken for private S3 bucket
```
<!-- endtab -->

<!-- tab docker-compose.yaml -->
```yaml
version: '3.7'

services:
  verdaccio:
    build: .
    image: verdaccio
    restart: always
    labels:
      - traefik.http.routers.verdaccio.rule=Host(`verdaccio.yourdomain.com`)
      - traefik.http.routers.verdaccio.entrypoints=websecure
      - traefik.http.routers.verdaccio.service=verdaccio
      - traefik.http.services.verdaccio.loadbalancer.server.port=4873
    volumes:
      - storage:/verdaccio/storage
  minio:
    image: minio/minio
    restart: always
    labels:
      - traefik.http.routers.minio.rule=Host(`minio.yourdomain.com`)
      - traefik.http.routers.minio.entrypoints=websecure
      - traefik.http.routers.minio.service=minio
      - traefik.http.services.minio.loadbalancer.server.port=9000
    command: server /data
    environment:
      - MINIO_ROOT_USER=pEPnuTGrb7fKm7bdFEcydE85ZXNrzKr4
      - MINIO_ROOT_PASSWORD=k5K8RVMpBeRDtiTxEvWk9oFWTySYYwwP
    volumes:
      - minio-data:/data

volumes:
  storage:
  minio-data:
```
<!-- endtab -->
{% endtabs %}

最后，使用下面命令启动服务：

```bash
docker-compose up -d --build
```

## Usage

```bash
# npm设置源
npm set registry https://verdaccio.yourdomain.com/
# 注册账号
npm adduser --registry https://verdaccio.yourdomain.com/
# 修改密码
npm profile set password --registry https://verdaccio.yourdomain.com/

# 登陆
npm login --registry https://verdaccio.yourdomain.com/
# 发布
npm publish --registry https://verdaccio.yourdomain.com/
```

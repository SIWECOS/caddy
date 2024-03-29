# caddy

![MicroBadger Size](https://img.shields.io/microbadger/image-size/siwecos/caddy.svg?style=for-the-badge)
![Docker Pulls](https://img.shields.io/docker/pulls/siwecos/caddy.svg?style=for-the-badge)
[![Main Project](https://img.shields.io/badge/Main%20Project-abiosoft%2Fcaddy--docker-blue.svg?style=for-the-badge)](https://github.com/abiosoft/caddy-docker)

A [Docker](https://docker.com) image for [Caddy](https://caddyserver.com). This image includes [cors](https://caddyserver.com/docs/http.cors), [ipfilter](https://caddyserver.com/docs/http.ipfilter), [realip](https://caddyserver.com/docs/http.realip), [expires](https://caddyserver.com/docs/http.expires) and [cache](https://caddyserver.com/docs/http.cache) plugins.

Plugins can be configured via the [`plugins` build arg](#custom-plugins).

Check [abiosoft/caddy:builder](https://github.com/abiosoft/caddy-docker/blob/master/BUILDER.md) for generating cross-platform Caddy binaries.

### License

This image is built from [source code](https://github.com/mholt/caddy). As such, it is subject to the project's [Apache 2.0 license](https://github.com/mholt/caddy/blob/baf6db5b570e36ea2fee30d50f879255a5895370/LICENSE.txt), but it neither contains nor is subject to [the EULA for Caddy's official binary distributions](https://github.com/mholt/caddy/blob/545fa844bbd188c1e5bff6926e5c410e695571a0/dist/EULA.txt).

Thanks to the original author @abiosoft for providing the [abiosoft/caddy-docker](https://github.com/abiosoft/caddy-docker) repository!

### Let's Encrypt Subscriber Agreement

Caddy may prompt to agree to [Let's Encrypt Subscriber Agreement](https://letsencrypt.org/documents/2017.11.15-LE-SA-v1.2.pdf). This is configurable with `ACME_AGREE` environment variable. Set it to true to agree. `ACME_AGREE=true`.


## Getting Started

```sh
$ docker run -d -p 2015:2015 siwecos/caddy
```

Point your browser to `http://127.0.0.1:2015`.

> Be aware! If you don't bind mount the location certificates are saved to, you may hit Let's Encrypt rate [limits](https://letsencrypt.org/docs/rate-limits/) rending further certificate generation or renewal disallowed (for a fixed period)! See "Saving Certificates" below!

### Saving Certificates

Save certificates on host machine to prevent regeneration every time container starts.
Let's Encrypt has [rate limit](https://community.letsencrypt.org/t/rate-limits-for-lets-encrypt/6769).

```sh
$ docker run -d \
    -v $(pwd)/Caddyfile:/etc/Caddyfile \
    -v $HOME/.caddy:/root/.caddy \
    -p 80:80 -p 443:443 \
    abiosoft/caddy
```

Here, `/root/.caddy` is the location _inside_ the container where caddy will save certificates.

Additionally, you can use an _environment variable_ to define the exact location caddy should save generated certificates:

```sh
$ docker run -d \
    -e "CADDYPATH=/etc/caddycerts" \
    -v $HOME/.caddy:/etc/caddycerts \
    -p 80:80 -p 443:443 \
    abiosoft/caddy
```

Above, we utilize the `CADDYPATH` environment variable to define a different location inside the container for
certificates to be stored. This is probably the safest option as it ensures any future docker image changes don't interfere with your ability to save certificates!

## Usage

#### Paths in container

Caddyfile: `/etc/Caddyfile`

Sites root: `/srv`

### Let's Encrypt Auto SSL

**Note** that this does not work on local environments.

Use a valid domain and add email to your Caddyfile to avoid prompt at runtime.
Replace `mydomain.com` with your domain and `user@host.com` with your email.

```
mydomain.com
tls user@host.com
```

## Credits
Thanks to the original author @abiosoft for providing the [abiosoft/caddy-docker](https://github.com/abiosoft/caddy-docker) repository!

Thanks to @mholt and the caddy community for creating [Caddy](https://caddyserver.com)

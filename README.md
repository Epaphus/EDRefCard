# Superseded
This repo is now superseded by [this one](https://github.com/brammmers/edrefcard2) and will soon be archived.

# Build status
* live  [![Build Status](https://travis-ci.org/richardbuckle/EDRefCard.svg?branch=live)](https://travis-ci.org/richardbuckle/EDRefCard)  [![Coverage Status](https://coveralls.io/repos/github/richardbuckle/EDRefCard/badge.svg?branch=live)](https://coveralls.io/github/richardbuckle/EDRefCard?branch=live)
* beta  [![Build Status](https://travis-ci.org/richardbuckle/EDRefCard.svg?branch=beta)](https://travis-ci.org/richardbuckle/EDRefCard)  [![Coverage Status](https://coveralls.io/repos/github/richardbuckle/EDRefCard/badge.svg?branch=beta)](https://coveralls.io/github/richardbuckle/EDRefCard?branch=beta)
* dev [![Build Status](https://travis-ci.org/richardbuckle/EDRefCard.svg?branch=dev)](https://travis-ci.org/richardbuckle/EDRefCard)  [![Coverage Status](https://coveralls.io/repos/github/richardbuckle/EDRefCard/badge.svg?branch=dev)](https://coveralls.io/github/richardbuckle/EDRefCard?branch=dev)

# Purpose
Elite: Dangerous has a great many command bindings to learn. To help with that, EDFRefCard generates a printable reference card from your Elite: Dangerous bindings file.

Currently hosted at [https://edrefcard.info/](https://edrefcard.info/).

# Dependencies

* Python 3.6 or later
	* Python3 module `lxml`
	* Python3 module `wand`
	* Python3 module `pytest`
	* Python3 module `pytest-cov`
	* Python3 module `coveralls`

* ImageMagick v6 (at the time of writing python wand doesn't support ImageMagick v7)
	* you may need to configure the `MAGICK_HOME` env var to get `wand` to see the ImageMagick libraries.

# Installation in a web server

* Base the server on the `www` subdirectory of this repo.
* Check that your server is supplying the env vars `CONTEXT_DOCUMENT_ROOT` and `SCRIPT_URI` so that `Config.dirRoot` and `Config.webRoot` in `www/scripts/bindings.py` get set correctly. Apache 2 does this by default. Python's `cgi.test()` is useful here.
* Enable Apache's `rewrite` and `cgi` or `cgid` mods.
* Add redirects as follows (in Apache 2 notation):

```
RewriteEngine On
RewriteRule ^/list$ /scripts/bindings.py?list=all
RewriteRule ^/binds/(.+)$ /scripts/bindings.py?replay=$1
RewriteRule ^/configs/([a-z][a-z])([^/]+)$ /configs/$1/$1$2
RewriteRule ^/devices$ /scripts/bindings.py?devicelist=all
RewriteRule ^/device/(.+)$ /scripts/bindings.py?blocks=$1
```
* Certain web servers, including Apache 2 on Debian 9, are prone to set brain-dead IO encodings, such as ANSI_X3.4-1968. To fix this, add the following at the end of `/etc/apache2/apache2.conf`:

```
SetEnv PYTHONIOENCODING utf-8
```

# Docker

Build a docker container:
```
docker build -t edrefcard .
```

Run the docker container in the background, exposed on port 8080 on the host:
```
docker run -d --rm --name edrefcard -p 8080:80 edrefcard
```

EDRefCard can then be accessed at http://localhost:8080

# Credits

EDRefCard is derived with permission from code originally developed by CMDR jgm.

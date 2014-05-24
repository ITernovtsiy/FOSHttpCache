Nginx Configuration
===================

This chapter describes how to configure Nginx to work with the library.

* [Introduction](#introduction)
* [Purge](#purge)
* [Refresh](#refresh)

Introduction
------------

Below you will find detailed Nginx configuration recommendations for the
features provided by this library. The examples are tested with Nginx version
1.4.6. For a quick overview, you can also look at [the configuration that is
used for the library’s functional tests] (../tests/Functional/Fixtures/nginx/fos.conf).

Purge
-----

Nginx does not support [purge requests](invalidation-introduction.md#purge) out
of the box. There is the [ngx_cache_purge](https://github.com/FRiCKLE/ngx_cache_purge)
module that adds some support. However, the semantics are not the same as for
Varnish: **The Nginx *purge* only removes the requested page, but no variants**.

You could use the [*refresh*](#Refresh) method which is easier to set up and
provides the same invalidation semantics, additionally preparing the cache with
the new content.

Unfortunately, you need to compile Nginx yourself to add the module.
For more information:

* see [this tutorial](http://mcnearney.net/blog/2010/2/28/compiling-nginx-cache-purging-support/)
  by Lance McNearney
* on Debian systems, you can run [install-nginx.sh](../tests/install-nginx.sh)
  to compile nginx like done for this library.

**Note**: The Nginx *purge* does not remove variants, only the page matching
the request.

Please refer to the [ngx_cache_purge module documentation](https://github.com/FRiCKLE/ngx_cache_purge)
for more on configuring Nginx to support purge requests.

Refresh
-------

If you want to invalidate cached objects by [forcing a refresh](invalidation-introduction.md#refresh)
you have to use the built-in [proxy_cache_bypass](http://wiki.nginx.org/HttpProxyModule#proxy_cache_bypass)
operation.

There are many ways to have a request bypass the cache. This library uses a
custom HTTP header named `X-Refresh`.
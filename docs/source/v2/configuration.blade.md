---
title: Configuration
description: Configuring stancl/tenancy
extends: _layouts.documentation_v2
section: content
---

# Configuration {#configuration}

The `config/tenancy.php` file lets you configure how the package behaves.

> **Note:** If the `tenancy.php` file doesn't exist in your `config` directory, you can publish it by running `php artisan vendor:publish --provider='Stancl\Tenancy\TenancyServiceProvider' --tag=config`

### `storage_driver, storage` {#storage}

This lets you configure the driver for tenant storage, i.e. what will be used to store information about your tenants. You can read more about this on the [Storage Drivers]({{ $page->link('storage-drivers') }}) page.

Available storage drivers:
- `Stancl\Tenancy\StorageDrivers\RedisStorageDriver`
- `Stancl\Tenancy\StorageDrivers\Database\DatabaseStorageDriver`

### `tenant_route_namespace` {#tenant-route-namespace}

Controller namespace used for routes in `routes/tenant.php`. The default value is the same as the namespace for `web.php` routes.

### `exempt_domains` {#exempt-domains}

If a hostname from this array is visited, the `tenant.php` routes won't be registered, letting you use the same routes as in that file.

### `database` {#database}

The application's default connection will be switched to a new one — `tenant`. This connection will be based on the connection specified in `tenancy.database.based_on`. The database name will be `tenancy.database.prefix + tenant id + tenancy.database.suffix`.

You can set the suffix to `.sqlite` if you're using sqlite and want the files to be with the `.sqlite` extension. Conversely, you can leave the suffix empty if you're using MySQL, for example.

### `redis` {#redis}

If `tenancy.redis.tenancy` is set to true, connections listed in `tenancy.redis.prefixed_connections` will be prefixed with `config('tenancy.redis.prefix_base') . $id`.

> Note: You need phpredis. Predis support will dropped by Laravel on version 7.

### `cache` {#cache}

The `CacheManager` instance that's resolved when you use the `Cache` or the `cache()` helper will be replaced by `Stancl\Tenancy\CacheManager`. This class automatically uses [tags](https://laravel.com/docs/master/cache#cache-tags). The tag will look like `config('tenancy.cache.tag_base') . $id`.

If you need to store something in global, non-tenant cache, 

### `filesystem` {#filesystem}

The `storage_path()` will be suffixed with a directory named `config('tenancy.filesystem.suffix_base') . $id`.

The root of each disk listed in `tenancy.filesystem.disks` will be suffixed with `config('tenancy.filesystem.suffix_base') . $id`.

For disks listed in `root_override`, the root will be that string with `%storage_path%` replaced by `storage_path()` *after* tenancy has been initialized. All other disks will be simply suffixed with `tenancy.filesystem.suffix_base` + the tenant id.

Read more about this on the [Filesystem Tenancy]({{ $page->link('filesystem-tenancy') }}) page.
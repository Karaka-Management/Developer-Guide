# Cache

For caching the `CacheManager` provides access to the caching systems in place. Out of the box the CacheManager supports and automatically initializes either Redis or Memcached depending on the client configuration. The caching is not mandatory and therefore shouldn't be misused as in-memory database. It is not necessary to check if Redis or Memcached are available the CacheManager automatically handles the caching based on their existence.

## HTTP Cache

By default only stylesheets, js and layout images as well as module images are cached. Everything else is considered volatile and not cached. If a response specific response should be cached feel free to use the response header:

Example usage for 30 days caching:

```php
$response->setHeader('Cache-Control', 'Cache-Control: max-age=2592000');
```

In order to trigger a re-cache of stylesheets or js files make sure to update the version in the `Controller.php` file. This way version updates will result in a new virtual file uri and result in a re-cache.

Example usage:

```php
$head->addAsset(AssetType::JS, $request->uri->getBase() . 'Modules/Media/Controller.js?v=' . self::VERSION);
```

## System Cache

All cache systems implement very similar functionality. It is only due to different features of the underlying technology that some minor details are different between the different cache solutions.

The available cache solutions are:

* File Cache
* Redis Cache
* MemCached

The available features are as follows:

* set - Sets a cache value or overwrites it (in some cache implementations with expiration date)
* add - Adds a cache value if the key doesn't exists (in some cache implementations with expiration date)
* replace - Replaces a cache value if the key exists
* get - Gets the cache value by key
* delete - Deletes a cache key/value
* exists - Checks if a key/value exists
* increment - Increments a numeric value by a given amount
* decrement - Decrements a numeric value by a given amount
* rename - Renames a key
* getLike - Finds keys/values based on a regex pattern
* deleteLike - Deletes keys/values based on a regex pattern
* updateExpire - Updates the expire date (only in some cache implementations possible)
* flush - Empties the cache by expiration
* flushAll - Empties the complete cache
# Caching mechanism for the frontend

> Caching mechanism desired for the get apis for desired period of time

## Use case for caching

Cache is used to improve user experience by speeding up data loading and reducing HTTP requests or server calls from the browser. It stores frequently used API responses or data that rarely changes on the backend but needs to be frequently displayed on the frontend. <br>
This approach saves the time taken for server calls, where the request goes from the browser to the API, fetches data from the backend, and returns it to the UI.

By using caching in the browser, we can check if the data is already stored in the cache. If it is, the data is returned directly from the cache instead of making an API request. If not, an API call is made to fetch the data.

## Steps

1. Create a caching service which will contain the cache based function.

initialize a cache name in a variable `cache_name`. Its better to patch the cache name with the app version. So that we can clear the cache based on the app version and keep only the latest version of the cache.

```typescript
cache_name: any = "app-name-v" + environment.app_version;
```

Cache service will contain mainly 6 functions.

-   `clear_cache_files()`

```typescript
 async clear_cache_files() {
    caches
      .keys()
      .then((cache_names) => {
        cache_names.forEach((cache_name) => {
          caches.delete(cache_name);
        });
      })
      .catch((err) => {
        console.error('Error while clearing unused caches: ', err);
      });
  }

```

This function is to clear the cache files. This is used if we need to clear all the cached data in the browser. Like while particular press of a button or logout.

-   `clear_other_cache_file()`

```typescript
  async clear_other_cache_file(filename: string) {
    caches
      .keys()
      .then((cache_names) => {
        cache_names.forEach((cache_name) => {
          if (cache_name !== this.cache_name) {
            caches.delete(cache_name);
          }
        });
      })
      .catch((err) => {
        console.error('Error while clearing unused caches: ', err);
      });
  }
```

This function is to clear the other cache files that are not the current cache file or which are not with the current app version and doesn't match the filename.

-   `clear_cache_key()`

```typescript
  async clear_cache_key(cache_key: string) {
    const cache = await caches.open(this.cache_name);
    try {
      const keys = await cache.keys();
      for (const key of keys) {
        if (key.url.includes(cache_key)) {
          await cache.delete(key);
        }
      }
    } catch (error) {
      console.log('Error clearing cache:', error);
    }
  }
```

This function is to clear the cache with a particular cache key.This is used when we need to clear the cache for a particular url. For example, when we need to clear the cache for a particular press of a button or when we use a `put` or `post` api and need to get the latest data from the backend.

-   `get_cached_data()`

```typescript
  async get_cached_data(key: string) {
    const cache = await caches.open(this.cache_name);
    const response = await cache.match(key);
    if (response) {
      const cachedData = await response.json();
      return cachedData.data; // Return the actual data from cached object
    }
    return null; // Return null if no cached data is found
  }
```

This function is to get the cached data from the cache which is stored in the browser. This will get the data from the cache based on the key.

-   `set_cached_data()`

```typescript
  async set_cached_data(key: string, data: any) {
    const cache = await caches.open(this.cache_name);
    const cached_object = {
      data,
      timestamp: Date.now(),
    };
    const response = new Response(JSON.stringify(cached_object));
    await cache.put(key, response);
  }
```

This function is to set the cached data in the cache. This will set the data in the cache based on the key. It will also set the timestamp in the cache so that we can check if the cache is valid or not.

-   `is_cache_valid()`

```typescript
  async is_cache_valid(key: string, expiry_time: number) {
    const cache = await caches.open(this.cache_name);
    const keys = await cache.keys();

    // check if cache is full. Limit is 50 entries
    if (keys.length >= 50) {
      caches.delete(this.cache_name);
      return false;
    }

    const cached_data = await this.get_cached_data(key);
    if (cached_data) {
      const cache = await caches.open(this.cache_name);
      const cache_key_response = await cache.match(key);
      if (cache_key_response) {
        const cached_object = await cache_key_response.json();
        const timeDiff = Date.now() - cached_object.timestamp;
        return timeDiff < expiry_time; // Return true if cache is still valid
      }
    }
    return false; // Return false if no cache or expired
  }
```

This function is to check if the cache is valid or not. It will check if the cache is valid or not based on the key and expiry time. Based on that it will return true or false. Here the is one more check that is to check **if the cache is full or not**. If it is full then we will delete the cache and return false.

2. Use the caching service in the other services where we need to cache the data or get the data from the cache.

> Ideally used in the get api function where we need to get the data from the cache or from the backend.

```typescript
get_api(): Observable<any> {
	let apiUrl = 'https://api.example.com/getDataById';
	const cacheKey = apiUrl;
	return new Observable((observer) => {
  	this.cache.is_cache_valid(cacheKey, 30000).then((isValid) => {
    	console.log('isValid --', isValid);
    	if (isValid) {
      	console.log('returning from cache');
          this.cache.get_cached_data(cacheKey).then((cachedData) => {
        	observer.next(cachedData); // Return cached data
        	observer.complete();
      	});
    	} else {
      	this.http
        	.get<any>(apiUrl, {
          	headers: new HttpHeaders({
            	// Add additional headers if needed
          	}),
        	})
        	.pipe(
          	tap((data) => {
            	// Cache the response
            	console.log('caching data', data);
                this.cache.set_cached_data(cacheKey, data);
          	})
        	)
        	.subscribe((data) => {
          	observer.next(data); // Return fresh data
          	observer.complete();
        	});
    	}
  	});
	});
}
```

In this example, the function first checks whether the API response is already stored in the cache. If it is, it retrieves the cached data. Otherwise, it makes the API call and stores the response in the cache for future use

## Delete the other cache files during app initialization

To delete old cache files in `app.module.ts`, add a function that is called when the app is loaded. This function will call the `clear_cache_file()` function from the service.

```typescript
export function clear_cache(cacheService: CacheService) {
    return () => cacheService.clear_other_cache_file("index.html");
}
```

In the `providers` array, add the following code:

```typescript
{
  provide: APP_INITIALIZER,
  useFactory: clear_cache,
  deps: [CacheService],
  multi: true // Allows multiple initializers to run
}
```

## Delete the cache files based on the app version when required.

This is straightforward. Just add the following code where ever its required to delete the cache files.

```typescript
this.cache.clear_cache_files();
```

> Don't forget to inject the service. If angular version is less that 16 use constructor to inject.

By generalizing the cache function to store only the desired APIs and clear old cached data, this approach ensures that the cache works efficiently and only for the relevant duration.

<br>
<br>

:artist: [Vivek V Pai](https://www.linkedin.com/in/vivek-v-pai-6b674b1b8/)

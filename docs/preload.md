"Preloading" means starting to load the next page before it is displayed at the end of the list, usually triggered by the logic of "pull up to load more." The prerequisite is that there is actually a next page available for the current list.

"Prefetching" refers to preloading in advance. It is similar to preloading as mentioned above. If you're not familiar with the term "pulling," please refer to the [Upfetch](upfetch.md) documentation.

## Preload Index for Current List

BRV enables preloading/prefetching by default. You can control when the preloading/prefetching starts by specifying the member property `preloadIndex`.

> The same property is used for both preloading and prefetching. When `preloadIndex` is set, both preloading and prefetching will take effect.

The default value is 3.

```kotlin
var preloadIndex = 3
```

## Global Preload Index

You can set the default value for the preload index using `PageRefreshLayout.preloadIndex`. This way, all lists will start preloading/prefetching from the specified index by default.
## 1.4.1
- feat: Added `copyType` method to preserve generics in collections, avoiding ambiguity in `addType` for distinguishing List generics.

## 1.4.0
- refactor: If `models` is null, `mutable` returns an empty collection.
- refactor: Removed `isNetworkingRetry`.
- refactor: Removed deprecated methods.
- upgrade: Upgraded StateLayout to version 1.4.1.
- fix: #200 Dragging the first item would quickly skip the list.
- feat: Added support for repeating animations in the list.

## 1.3.90
- feat: #324 Grid divider spacing now supports item-based spacing.
- refactor: Removed the `setBackground()` method from `DefaultDecoration.kt` (can be replaced with the background color of the `rv`).
- fix: Fixed grid divider spacing issue.

## 1.3.89
- upgrade: Upgraded StateLayout to version 1.3.13.
- sample: Used mock data.
- sample: Added nested list example.
- sample: Added home layout example.
- sample: Avoided duplicate dialog display.

## 1.3.88
- fix: #317 Unable to drag to adjacent positions of an item (when drag state is disabled).
- fix: #305 Assigning new data to the list can restore the expanded state.
- pref: Improved expand/collapse logic.
- sample: #313 Fixed crash when deleting nested groups.
- sample: Fixed something.

## 1.3.87
- fix: #311 `notifyItemChanged` triggering `onRefresh`.
- pref: Optimized `dataBinding` to inflate layout only once when using non-layout bindings.
- feat: Added `page_upFetchEnabled` property.
- upgrade: Upgraded StateLayout to version 1.3.12.

## 1.3.86
- fix: `PageRefreshLayout.isNetworkingRetry` noop.
- feat: #292 Added `refreshEnableWhenEmpty` and `refreshEnableWhenError` properties to control pull-to-refresh when the default page is empty.
- pref: #298 `onCreate` now supports `itemViewType` values.

## 1.3.85
- feat: #286 Added `getBinding()` to retrieve the ViewBinding instance.

## 1.3.84
- fix: `setDifferModel` doesn't support inheritance from `ItemExpand` group collection data.
- fix: #281 `onPayload` didn't pass payload data collection.

## 1.3.83
- fix: Refreshing data caused single selection to fail.
- upgrade: Upgraded StateLayout to version 1.3.11 to fix memory leaks in `FadeStateChangedHandler`.

## 1.3.82
- fix: [#263](https://github.com/liangjingkanji/BRV/issues/263) Fixed custom swipe view reuse issue with `android:tag="swipe"`.

## 1.3.81
Fixed issue where removing items with headers during swipe deletion caused index out of bounds error.

## 1.3.80
pref: `setRetryIds` now uses the most recent `showLoading` tag when clicking on retry.

## 1.3.79
- fix: Fixed grid divider dynamic `spanSize` spacing loss.
- feat: `findView` now supports nullable types.
- feat: Added `dividerSpace` function.
- feat: Upgraded StateLayout to version 1.3.8, introducing `isNetworkingRetry` to disable loading default page when there is no network.
- pref: Added logging for data binding failures.
- refactor: Deprecated `page` function and added

## 1.3.79
- fix: Fixed grid divider dynamic `spanSize` spacing loss.
- feat: Added `findView` method that supports nullable types.
- feat: Added `dividerSpace` function.
- feat: Upgraded StateLayout to version 1.3.8, introducing `isNetworkingRetry` to disable displaying the default loading page when there is no network.
- pref: Added logging for data binding failures.
- refactor: Deprecated the `page` function and added `pageCreate` function.

## 1.3.78
- feat: Added `page_rv` attribute to `PageRefreshLayout` to specify the list.
- feat: Added `page_state` layout attribute to `PageRefreshLayout` to specify the default page.

## 1.3.77
- feat: Upgraded StateLayout to version 1.3.6.
- feat: The `ACCESS_NETWORK_STATE` permission (to avoid displaying the loading default page without a network) can be safely removed, allowing the library to run with zero permissions.

## 1.3.76
- fix: Fixed inconsistency in item size when grid and spacing directions are the same.

## 1.3.75
- fix: Replaced `showEmpty` with "No more loading" message.
- fix: Improved index checking when using `addData` in `PageRefreshLayout` to avoid invalid state checking.

## 1.3.74
- fix: Fixed calculation error for group depth.

## 1.3.73
- fix: Upgraded StateLayout to version 1.3.5.

## 1.3.72
- refactor: Upgraded StateLayout to version 1.3.4.
- feat: Added `refreshing` parameter to `refresh` function.

## 1.3.71
Fixed issue with `addModels` using index for partial updates.

## 1.3.70
- feat: Added `ItemStableId` to fix list IDs.
- feature #186: Added index insertion for `addModels`.
- Moved `ItemDepth` to a new position.

## 1.3.69
Fixed #184: Fixed `setCheckableType`.

## 1.3.68
- Fixed #182.
- feat: Added singleton `BindingAdapter.modelId`.
- Increased deprecation level for APIs.

## 1.3.67
- feat: Added `ItemAttached`.

## 1.3.66
- upgrade: Upgraded StateLayout to version 1.3.3.
- feat: `showLoading` is only displayed as LOADING when there is network connectivity, without affecting `onRefresh`.

## 1.3.64
- upgrade: Upgraded StateLayout to version 1.3.1.
- feat: Added `StateChangedHandler` for custom default page switching.
- feat: Added `FadeStateChangedHandler` for fade-in/fade-out default page transitions.
- feat: Added `getBindingOrNull` method.

## 1.3.63
Fixed [#164](https://github.com/liangjingkanji/BRV/issues/164).

## 1.3.61
Fixed [#157](https://github.com/liangjingkanji/BRV/issues/157).

## 1.3.60
Optional DataBinding dependency.

## 1.3.58
Fixed [#141](https://github.com/liangjingkanji/BRV/issues/141).

## 1.3.57
The `hasMore` parameter of `PageRefreshLayout.showContent` now defaults to `true`, to avoid unexpected disabling of the load more behavior.

## 1.3.56
Fixed crash

during refresh.

## 1.3.55
Resolved issue with incorrect loading more behavior when calling `PageRefreshLayout.finishRefresh` instead of `SmartRefreshLayout.finishRefresh`.

## 1.3.54
- Fixed [#119](https://github.com/liangjingkanji/BRV/issues/119).
- Prevented duplicate expansion/collapse of groups.
- Fixed single expansion mode.

## 1.3.53
- Fixed [#107](https://github.com/liangjingkanji/BRV/issues/107).
- Fixed [#108](https://github.com/liangjingkanji/BRV/issues/108).

## 1.3.52
Fixed issue with multiple loading pages being displayed, causing `showError` to be ineffective.

## 1.3.51
- fix: Fixed incorrect item display when dragging and dropping.
- feat: Added support for specifying `onClick/onFastClick/onLongClick` using ID directly.
```kotlin
rv.linear().setup {
    addType<SimpleModel>(R.layout.item_simple)
    R.id.tv_simple.onClick {
        toast("Clicked Text")
    }
}.models = getData()
```

## 1.3.50
Updated internal dependency on SmartRefreshLayout to the latest version `2.0.5`.
```groovy
api 'io.github.scwang90:refresh-layout-kernel:2.0.5'
api 'io.github.scwang90:refresh-header-material:2.0.5'
api 'io.github.scwang90:refresh-footer-classics:2.0.5'
```
Note that if you have added any additional SmartRefreshLayout refresh headers, you also need to update them to the latest `mavenCentral()` version.

For more details, refer to [#85](https://github.com/liangjingkanji/BRV/issues/85).

## 1.3.40
- fix: Fixed issue with pull-to-refresh not working.
- feat: Added examples for grid grouping and drag grouping.
- feat: Optimized list load updates.
- Modified `addModels` and `addData` to maintain the same object in `models`.
- Updated example code.

## 1.3.39
Exposed callback interface for comparing data changes.

## 1.3.38
- feat: Added `setDifferModels` function for data comparison and refresh. BRV already supports the official comparison refresh solution, but this enhancement optimizes the process.
- feat: Added `PageRefreshLayout.refreshing` to load the default page only on the first data load and use silent loading for subsequent loads.

## 1.3.37
- fix: Fixed reversed default page issue in UpFetch mode.
- fix: Fixed divider display issue in `reverseLayout`.
- feat: Added `ItemDepthUtils` to simplify getting the depth of grouped items.

## 1.3.36
- fix: Fixed divider display issue in UpFetch mode.

## 1.3.35
- feat: Added support for polymorphic interfaces in the `addType` function for multi-type lists.
- fix: Fixed content reversal issue on Activity with UpFetch loading.

## 1.3.34
Fixed issue with duplicate data when expanding nested groups (caused by mutable child lists).

## 1.3.33
- fix: Fixed issue with enabling only load more.
- fix: Improved performance of parent item lookup with `findParentPosition`.

## 1.3.32
- feat: Allow overriding of `BindingAdapter`.
- fix: Fixed incorrect determination of no more pages when loading more.

## 1.3.31
Fixed crash issue with default page in `PageRefreshLayout`.

## 1.3.30
1. Upgraded dependency on SmartRefreshLayout to version `2.0.3`.
2. Upgraded dependency on StateLayout to version `1.2.0`.
3. Added `onContent` listener for default page.

## 1.3.29
Hidden internal function `throttleClick`.

## 1.3.28
Fixed issue with `onFastClick` not working.

## 1.3.27
Configurable global interval for [click debouncing](click.md#_4).

## 1.3.26
- fix: Fixed incorrect parameter in `showLoading` default page.
- feat: Added `isSampleGroup` function to determine if two items are in the same group.
- feat: Improved performance of parent item lookup.

## 1.3.24
Fixed issue with dynamic dividers when adding data for partial refresh.

## 1.3.22
Deprecated some functions to reduce the need for checking IDs after adding click events.

| Deprecated Function | Replacement |
|-|-|
| `addFastClickable` | Replaced with `onFastClick` |
| `addClickable` | Replaced with `onClick` |
| `addLongClickable` | Replaced with `onLongClick` |

## 1.3.21
Added support for directly calling `onClick/onFastClick/onLongClick` using ID.
```kotlin
rv.linear().setup {
    addType<SimpleModel>(R.layout.item_simple)
    R.id.tv_simple.onClick {
        toast("Clicked Text")
    }
}.models = getData()
```

## 1.3.20
Fixed issue where singleton default page was not overriding global default page.



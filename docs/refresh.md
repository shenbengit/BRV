The content of this chapter may seem extensive, but the code is actually concise. The detailed explanations are omitted to avoid redundancy.

<img src="https://i.loli.net/2021/08/14/lV4ktFRAweYorsC.gif" width="250"/>

[SmartRefreshLayout](https://github.com/scwang90/SmartRefreshLayout) is currently the most powerful refresh framework for Android in terms of extensibility. BRV's pull-to-refresh and load-more functionalities are built on top of SmartRefreshLayout, supporting all its features and adding new ones.
<br>

> If you need more customization options for pull-to-refresh or load-more, please refer to the documentation of [SmartRefreshLayout](https://github.com/scwang90/SmartRefreshLayout). <br>
The `PageRefreshLayout` in this library inherits from `SmartRefreshLayout`, so it has all its features.

<br>
This library includes the following basic dependencies of SmartRefreshLayout, so there's no need to include them again:

```groovy
api 'io.github.scwang90:refresh-layout-kernel:2.0.5'
api 'io.github.scwang90:refresh-header-material:2.0.5'
api 'io.github.scwang90:refresh-footer-classics:2.0.5'
```

To specify custom refresh headers and footers for SmartRefreshLayout, you need to include the respective libraries (that's how SmartRefreshLayout is designed):

Optional configurations for refresh headers and footers:

```groovy
implementation  'io.github.scwang90:refresh-layout-kernel:2.0.5'      // Core dependency (must include)
implementation  'io.github.scwang90:refresh-header-classics:2.0.5'    // Classic refresh header
implementation  'io.github.scwang90:refresh-header-radar:2.0.5'       // Radar refresh header
implementation  'io.github.scwang90:refresh-header-falsify:2.0.5'     // Falsify refresh header
implementation  'io.github.scwang90:refresh-header-material:2.0.5'    // Material refresh header
implementation  'io.github.scwang90:refresh-header-two-level:2.0.5'   // Two-level refresh header
implementation  'io.github.scwang90:refresh-footer-ball:2.0.5'        // Ball pulse load-more
implementation  'io.github.scwang90:refresh-footer-classics:2.0.5'    // Classic load-more
```

## Initialization
The refresh layout requires initialization first, and it's recommended to do it in the `Application` class.

```java
SmartRefreshLayout.setDefaultRefreshHeaderCreator { context, layout -> MaterialHeader(this) }
SmartRefreshLayout.setDefaultRefreshFooterCreator { context, layout -> ClassicsFooter(this) }
```

## PageRefreshLayout

This control extends from `SmartRefreshLayout` and adds the following features:

1. Simplified functions
2. Fine-tuned details
3. Empty state
4. Pagination loading
5. Pull-to-load-more
6. Pre-loading / Pre-pulling

### Declaration

There are two ways to create a `PageRefreshLayout`, but it's recommended to wrap it in a layout.

=== "Wrap in layout"

```xml
<com.drake.brv.PageRefreshLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:id="@+id/page"
    app:stateEnabled="true"
    android:layout_height="match_parent"
    tools:context="com.drake.brv.sample.fragment.Refresh

Fragment">

<androidx.recyclerview.widget.RecyclerView
    android:id="@+id/rv"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />

</com.drake.brv.PageRefreshLayout>
```

=== "Wrapped in code"
    It is only recommended to use layout creation, which can keep the project code readable and avoid unnecessary problems

```kotlin
val page = rv.pageCreate()
```

### Create the List

```kotlin
rv.linear().setup {
    addType<Model>(R.layout.item_simple)
    addType<DoubleItemModel>(R.layout.item_full_span)
}

page.onRefresh {
    postDelayed({
        val data = getData()
        addData(data) {
            index < total
        }
    }, 2000)
}.autoRefresh()
```

1. `onRefresh` is the function that will be executed every time the list is refreshed or loaded more.

### Listen for State Changes

```kotlin
// Pull-to-refresh
page.onRefresh {
	// You can perform network requests or other asynchronous operations here
}

// Load more
page.onLoadMore {
	// You can perform network requests or other asynchronous operations here
}
```

1. If you don't call `onLoadMore`, the pull-to-load-more functionality will also execute the `onRefresh` function. In most cases, pull-to-refresh and load more use the same API but with different page values.

### Trigger Refresh

There are three functions to trigger the refresh state (all of them will invoke the `onRefresh` function):

| Function    | Description                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------|
| autoRefresh | Show the pull-to-refresh animation                                                                       |
| showLoading | Show the loading state with a custom layout                                                              |
| refresh     | Refresh silently without animation                                                                       |
| refreshing  | Equivalent to `showLoading` for the first time. After loading is complete, it's equivalent to `refresh`. |

> All three trigger methods will reset the index to `startIndex`. The index is the default field for page increment. You will see how to use this field later.

### Empty State

The `PageRefreshLayout` contains a `StateLayout` to handle the empty state.

Control the state of the empty state layout (usually handled automatically by the framework):

| Function    | Description                  |
|-------------|------------------------------|
| showLoading | Show the loading empty state |
| showEmpty   | Show the empty empty state   |
| showError   | Show the error empty state   |
| showContent | Show the content view        |

#### Global Empty State Configuration

Global empty state configuration is shared with `StateLayout` because `PageRefreshLayout` embeds `StateLayout`.

```kotlin
/**
 * Recommended to configure the global empty state in the Application class.
 * For more details, refer to https://github.com/liangjingkanji/StateLayout.
 */
StateConfig.apply {
  emptyLayout = R.layout.layout_empty
  errorLayout = R.layout.layout_error
  loadingLayout = R.layout.layout_loading

  onLoading {
    // This callback is triggered when the loading layout is created. You can customize animations or click events here.
  }
}
```

#### Singleton Empty State Configuration

Singleton empty state means that for a specific layout, you want to use a different empty state layout instead of the global configuration. You don't need to specify all of them; you can choose to specify only the loading or error layout.

=== "XML Declaration"

```xml
<com.drake.brv.PageRefreshLayout
    .....
    app:error_layout="@layout/layout_error"
    app:empty_layout="@layout/layout_empty"
    app:loading_layout="@layout/layout_loading">

    <!--RecyclerView code-->

</com.drake.brv.PageRefreshLayout>
```

=== "Code Declaration"

```kotlin
page.apply {
    loadingLayout = R.layout.layout_loading
    emptyLayout = R.layout.layout_empty
    // errorLayout = R.layout.layout_error
}
```

By default, the empty state will be used. If you

have set the global empty state and don't want to use it at this moment, you can use the `stateEnabled` property or function.

As the header layout is part of the list and the empty state covers the entire list, if you want to use the empty state without affecting the header layout, you can use `CoordinatorLayout`.

> Note: Using `NestedScrollView` to nest the RecyclerView will load all items at once, which consumes more memory. However, nesting the RecyclerView inside a `CoordinatorLayout` won't have this issue.

### Refreshing Data

As mentioned earlier, `PageRefreshLayout` supports automatic pagination. You don't need to use the `rv.models` function to set the data; instead, you can use `addData`.

```kotlin
pageLayout.onRefresh {
    scope {
        val data = Get<String>("/path").await()
        addData(data.list) {
            adapter.itemCount < data.count
        }
    }
}
```

In most cases, the backend defines the first page as 1, but sometimes it can be 0. You can set the initial value of the `index` field (the field representing the page) in the `onCreate` method of your `Application` class.

```kotlin
class App : Application() {
    override fun onCreate() {
        super.onCreate()
        PageRefreshLayout.startIndex = 1
    }
}
```

In the code snippet above, I used another open-source project of mine, [Net](https://github.com/liangjingkanji/Net), for network requests. It can be configured together with BRV.

> If `PageRefreshLayout` is not directly wrapping the RecyclerView, you need to pass the `adapter` parameter to the `addData` function in order to use automatic pagination. Otherwise, an exception will be thrown.

## Disable Pull-to-Refresh in Empty State

Control the PageRefreshLayout's ability to handle pull-to-refresh gestures when the empty state is displayed.

| Function               | Description                                              |
|------------------------|----------------------------------------------------------|
| refreshEnableWhenEmpty | Enable pull-to-refresh when the empty state is displayed |
| refreshEnableWhenError | Enable pull-to-refresh when the error state is displayed |

> The corresponding companion object properties are used for global configuration, while the object properties of PageRefreshLayout are used for singleton configuration.

If you need more complex logic, it is recommended to use `PageRefreshLayout.setEnableRefresh()` in the empty state callback to have more control.
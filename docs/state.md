The default page is very important for a user-friendly application.

BRV integrates a highly efficient default page library called [StateLayout](https://github.com/liangjingkanji/StateLayout) to implement default pages for lists.

> StateLayout is already embedded in the BRV library, so there is no need to depend on StateLayout separately. If your list includes both pull-to-refresh and load-more functionality, I recommend using [PageRefreshLayout](refresh.md) instead of StateLayout.

Main Features:

- [x] Elegant function design
- [x] Partial default pages
- [x] Layout or code declaration
- [x] Global/singleton configuration
- [x] Listening to default page display
- [x] Custom animations
- [x] Multiple state default pages
- [x] Network request callbacks
- [x] Passing any object as a tag
- [x] Quick configuration for click-to-retry
- [x] Asynchronous threads
- [x] Immediate display of error default page when there is no network
- [x] Automatic display of list default page when used with lists
- [x] Automatic display of default page when used with network requests

![StateLayout Demo](https://i.loli.net/2021/08/14/cliN9VtnAfjb1Z4.gif)

## Usage

Step 1: Initialize the default page in your Application class.

```kotlin
StateConfig.apply {
    emptyLayout = R.layout.layout_empty
    errorLayout = R.layout.layout_error
    loadingLayout = R.layout.layout_loading
}
```

Step 2: Create the default page.

=== "Layout Creation"
```xml
<com.drake.statelayout.StateLayout
android:id="@+id/state"
android:layout_width="match_parent"
android:layout_height="match_parent">

        <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/rv"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />
    </com.drake.statelayout.StateLayout>
```
=== "Code Creation"
It is recommended to create the StateLayout in XML layout to maintain code readability and avoid unnecessary issues for better performance.
```kotlin
val state = stateCreate() // Create the default page function directly in the Activity/Fragment. `rv.stateCreate()` can also be used, but it is strongly discouraged.
```

> For CoordinatorLayout+ViewPager, the root layout of the default page XML must be NestedScrollView in order to scroll correctly after displaying the default page.

Step 3: Create the list.

```kotlin
rv.linear().setup {
    addType<Model>(R.layout.item_simple)
    addType<DoubleItemModel>(R.layout.item_full_span)
}.models = getData()
```

Step 4: Show the default page.

```kotlin
state.showLoading()  // Show loading state
state.showContent() // Show content
state.showError() // Show error state
state.showEmpty() // Show empty state
```

## StateLayout

StateLayout is a highly recommended default page library. BRV internally integrates it to provide default pages for lists to developers.

If you want to customize the default page animations and listen to the lifecycle of the default page, I recommend reading the following documentation:

- [GitHub](https://github.com/liangjingkanji/StateLayout/)
- [Documentation](https://liangjingkanji.github.io/StateLayout)

## Skeleton Animation

Skeleton animation refers to the animation or image corresponding to the layout.

BRV implements skeleton animation using StateLayout. You can refer to the [Skeleton Animation](https://liangjingkanji.github.io/

StateLayout/skeleton/) documentation for specific implementation code examples.

For code examples, you can refer to the [sample](https://github.com/liangjingkanji/BRV/blob/857e33e2bbc80bbdb9b5dec364012ff353b74d5a/sample/src/main/java/com/drake/brv/sample/ui/fragment/SkeletonFragment.kt) in the BRV repository.

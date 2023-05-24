The provided code demonstrates how to add header and footer layouts to a RecyclerView using the BRV library:

1. Both the header and footer layouts are considered as items within the RecyclerView, so when calculating the position, you need to take them into account.
2. You need to use the `addType` function to add types for the header and footer layouts.

Here's an example of how to add header and footer layouts using BRV:

```kotlin
binding.rv.linear().setup {
    addType<Model>(R.layout.item_simple)

    /**
     * The BRV data set = Header + Footer + Models
     * So essentially, the header and footer layouts are just another group of multiple types.
     * Separating them out is to facilitate the replacement of Models without affecting the Header and Footer.
     */

    addType<Header>(R.layout.item_header)
    addType<Footer>(R.layout.item_footer)
}.models = getData()

binding.rv.bindingAdapter.addHeader(Header(), animation = true)
binding.rv.bindingAdapter.addFooter(Footer(), animation = true)
```

If you don't want the default page to overlap with the header, you can use the following solutions:

1. If you don't want the default page to cover the header, you can use the `CoordinatorLayout` to implement the header effect. The default page functionality of `PageRefreshLayout` can cause the entire list to be covered by the default page.
2. If you only want to prevent the header or footer from interfering with BRV, you can use [ConcatAdapter](https://developer.android.com/reference/androidx/recyclerview/widget/ConcatAdapter) to achieve it.
3. You can use a `ScrollView/NestedScrollView` to nest the RecyclerView to implement the header, but it will cause the RecyclerView to lose the item reuse effect. If there is a large amount of data in the list, it may cause lag (if the data volume is small, this can be ignored).

## Partial Empty State for the List

If you are using the `CoordinatorLayout` solution to address the issue of the empty state overlapping the header, but you want the pull-to-refresh to start from the top of the page, you can refer to the implementation code in the demo's `PageNestedHeaderFragment`. Manually specify the empty state to avoid overlapping the header.

```xml
<com.drake.brv.PageRefreshLayout
    android:id="@+id/page"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:page_rv="@id/rv"
    app:page_state="@id/state"/>
```

1. Use the `app:page_rv` attribute to specify the nested RecyclerView.
2. Use the `app:page_state` attribute to specify the nested empty state.

## Functions

| Function | Description |
|-|-|
| addHeader / addFooter | Add a header layout or a footer layout. |
| removeHeader / removeFooter | Remove the header layout or the footer layout. |
| removeHeaderAt / removeFooterAt | Remove the header layout or the footer layout at the specified index. |
| clearHeader / clearFooter | Clear all header layouts or footer layouts. |
| isHeader / isFooter | Check if the specified index is a header layout or a footer layout. |
| headerCount / footerCount | Get the count of header layouts or footer layouts. |
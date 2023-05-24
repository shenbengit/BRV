First of all, I assume you have already read the [lifecycle](lifecycle.md) documentation on the basics of RecyclerView. Under normal circumstances, the list should not experience lag even without implementing the optimization methods discussed in this chapter.

> BRV does not have any performance impact on RecyclerView. The optimization points mentioned here are specific to RecyclerView. For other optimizations, please refer to additional resources or suggestions.

Performance optimization is mainly about avoiding time-consuming operations during list scrolling, and `onBind` is a callback that gets triggered frequently during the scrolling process.

## Main Thread Data Read/Write

SharePreferences (referred to as sp) is a built-in key-value storage tool in Android. Although it allows convenient data read/write on the main thread, its performance is not satisfactory. If you read large-sized sp data in `onBind`, it can lead to list lagging or even ANR (Application Not Responding) issues.

While many suggestions online recommend using MMKV as an alternative, I recommend using [Serialze](https://github.com/liangjingkanji/Serialize), which is a more efficient and convenient wrapper based on MMKV.

## Nested Lists

When using a RecyclerView within another RecyclerView, it is advisable to set the view for the nested RecyclerView in the `onCreate` callback (using `rv.setup`). This helps avoid excessive creation of RecyclerView instances of the same type, which can lead to increased memory consumption. The data for the nested RecyclerView can be bound in `onBind` using `rv.models`.

```kotlin
rv.linear().setup {
    addType<SimpleModel>(R.layout.item_simple)

    onCreate {
        val rv = findView<RecyclerView>(R.id.rv_check_mode)
        rv.linear().setup { // Setting the view in onCreate avoids repeated callbacks during list scrolling
            addType<NestedModel>(R.layout.item_simple_nested)
        }
    }

    onBind {
        val rv = findView<RecyclerView>(R.id.rv_check_mode)
        rv.models = getModel<Model>().listNested // Only onBind can access the data
    }
}
```

Since the nested RecyclerView cannot reuse items, it is recommended to use `recycledViewPool` for recycling. You can search for specific keywords for more details.

If the nested list is very simple, you may not need to consider reuse optimization. In fact, dynamically adding views using `addView` might be simpler than using a nested list.

## View Addition/Removal

To avoid frequent main thread operations such as `addView` and `removeView` on views, it is recommended to:

1. Use `visibility` to control view visibility.
2. Check if the view has already been added before adding it again. If the view is already added, assign the value instead of removing it.
3. If you are only adding icons, consider using [Spannable](https://github.com/liangjingkanji/spannable) to construct text with mixed graphics, and directly assign it to a TextView.

## Fast Scroll Throttling

The implementation principle is similar to automatic search throttling. Delay the data loading until a certain time has passed and the item is still visible on the screen. This is because during fast scrolling, most items are displayed on the screen for a very short time and do not require data loading.

```kotlin
data class SimpleModel(var name: String = "BRV") : ItemBind, ItemAttached {

    private var itemVisible: Boolean = false

    override fun onViewAttachedToWindow(holder: BindingAdapter.BindingViewHolder) {
        itemVisible = true
    }

    override fun onViewDetachedFromWindow(holder: BindingAdapter.BindingViewHolder) {
        itemVisible = false
    }

    override fun onBind(holder: BindingAdapter.BindingViewHolder) {
       

 holder.itemView.postDelayed({
            if (itemVisible) {
                // Trigger loading only if the item is still visible after 500ms
            }
        }, 500)
    }

}
```

If you use Glide for image loading, you can also refer to [Glide Preloading in RecyclerView](https://muyangmin.github.io/glide-docs-cn/int/recyclerview.html), which can significantly reduce the number of "loading" images visible to users when scrolling through an image list.

## Fixed Layout Optimization

If the width and height of all items in the list do not change dynamically due to adapter modifications, you can use `setHasFixedSize(true)` to reduce the number of layout calculations and improve performance.

## Unique Item Identifiers

Improve the performance of using `notifyDataSetChanged()` by assigning unique IDs to items in the list.

```kotlin
binding.rv.linear().setup {
    setHasStableIds(true) // Enable unique IDs
    addType<UserModel>(R.layout.item_user)
}.models = getData()
```

The data model should implement `ItemStableId`.

```kotlin
data class UserModel(var userId: Long) : ItemStableId {

    override fun getItemId(): Long {
        return userId // Return the unique ID in the list
    }
}
```

I hope these optimizations help improve the performance of your RecyclerView. Let me know if you have any more questions!
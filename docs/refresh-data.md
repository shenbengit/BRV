BRV does not have a custom RecyclerView, so data manipulation in BRV is done in the same way as with RecyclerView. For those who are not familiar with the basics of RecyclerView, you can read this chapter and then search online for more information.

The process of manipulating data in RecyclerView involves two steps:

1. Updating the collection (adding or removing elements).
2. Calling the corresponding `notify**()` methods to update the list.

However, in BRV, assigning values to `models` or using `addData()` will automatically call `notifyDataChanged()`, so there is no need to manually update the list. If you don't want the list to be automatically updated, you can use the `_data` field in the BindingAdapter.

> The data collection in BRV, whether it's `models` or `addData()`, is of type `List<Any?>` (a collection of any objects). So as long as it's a collection, it can be mapped to a list. <br>
> If the data doesn't meet the requirements of a collection (or any issues with the data), please handle the data accordingly.

## Adding Data

Using the built-in methods in BRV to add data will automatically refresh the UI.

```kotlin
rv.models = dataList // Automatically calls notifyDataSetChanged
rv.addModels(newDataList) // Automatically calls notifyItemRangeInserted, animation can also be disabled
binding.rv.addModels(newDataList, index = 3) // Add data after index 3
```

Code Example:
```kotlin
binding.rv.linear().setup {
    addType<SimpleModel>(R.layout.item_simple)
    onBind {
        findView<TextView>(R.id.tv_simple).text = getModel<SimpleModel>().name
    }
}.models = getData()

binding.rv.addModels(data, index = 3) // Add data
binding.rv.models = newDataList // Replace the original list with new data
```


> Remember that in Java/Kotlin, reference types are passed by reference. If both methods operate on the same data collection, it will cause issues with adding the same data to itself. This is a basic language syntax issue.

## Removing Data

Manipulating data in BRV is the same as in RecyclerView (after all, it's just a custom Adapter). To remove all data, you can assign `null` to `models`.

If you want to remove specific data, such as removing the second item:

```kotlin
rv.mutable.removeAt(2) // Remove the data first
rv.bindingAdapter.notifyItemRemoved(2) // Then refresh the list
```

If you are removing or adding/inserting multiple items, please use the corresponding `notifyItem**()` methods. You can find the specific methods at the end of this chapter or search online.

## Updating Data with Comparison

BRV can automatically use refresh animations based on the comparison of new and old data collections.

```kotlin
rv.setDifferModels(getRandomData())
```

This method internally uses the Android tool `DiffUtil` to compare and refresh the data. It supports synchronous and asynchronous thread comparison.
```kotlin
fun setDifferModels(newModels: List<Any?>?, detectMoves: Boolean = true, commitCallback: Runnable? = null)
```
> By default, data comparison uses the `equals` method. You can manually implement the `equals` method for your data to modify the comparison logic. It is recommended to define data as `data class`, as it will automatically generate `equals` based on the constructor parameters.

If you need to completely customize the comparison logic for data, you can implement the `ItemDifferCallback` interface.

```kotlin hl_lines="3"
rv.linear().setup {
    add

Type<SimpleModel>(R.layout.item_simple)
    itemDifferCallback = object : ItemDifferCallback {
        override fun areContentsTheSame(oldItem: Any, newItem: Any): Boolean {
            return if (oldItem is SimpleModel && newItem is SimpleModel) {
                oldItem.name == newItem.name
            } else super.areContentsTheSame(oldItem, newItem)
        }
    }
    // ...
}.models = getRandomData(true)
```

When using `setDifferModels` for comparison and refresh, if the same item refreshes with a blank animation, it means that `getChangePayload` returns null. Just return any object to disable it.

```kotlin
rv.linear().setup {
    // ...
    itemDifferCallback = object : ItemDifferCallback {
        override fun getChangePayload(oldItem: Any, newItem: Any): Any? {
            return true
        }
    }
}.models = getRandomData(true)
```

## Partial Refresh

To partially refresh a specific item or a batch of items, there are two approaches:

1. Using methods like `notifyItemChanged`, as mentioned above.
2. Utilizing the built-in feature of DataBinding (recommended), which provides the highest performance and convenience. The [Check Mode](https://github.com/liangjingkanji/BRV/blob/master/sample/src/main/java/com/drake/brv/sample/ui/fragment/CheckModeFragment.kt) in the demo is implemented using this method.

<br>
BRV supports data binding, and if the data model used by DataBinding extends `Observable`, the UI will be automatically updated.

```kotlin
data class CheckModel(var checked: Boolean = false, var visibility: Boolean = false) : BaseObservable()
```

Fields that can automatically update the UI can also be used without the data model itself extending `BaseObservable`.

- LiveData
- ObservableField

To automatically update LiveData fields, you need to configure the lifecycle for DataBinding, as LiveData observers require a lifecycle owner.
```kotlin
binding.lifecycleOwner = this
```

> These are the basics of using DataBinding. For more information, please read: [Comprehensive DataBinding Guide](https://juejin.cn/post/6844903549223059463)

## Refresh Methods

Here, we introduce the RecyclerView's official methods. The `BindingAdapter` in BRV inherits from `RecyclerView.Adapter`, so it naturally has access to the parent class's data refresh methods.
Since many developers often ask about this requirement, let's discuss it in detail.

```kotlin
class BindingAdapter : RecyclerView.Adapter<BindingAdapter.BindingViewHolder>()
```

| Refresh Method | Description |
|-|-|
| notifyDataSetChanged | Refreshes all data (without animation) |
| notifyItemChanged | Partially updates data |
| notifyItemInserted | Inserts an item |
| notifyItemMoved | Moves an item |
| notifyItemRemoved | Removes an item |
| notifyItemRangeChanged | Partially updates items within a specified range |
| notifyItemRangeInserted | Inserts items within a specified range |
| notifyItemRangeRemoved | Removes items within a specified range |

You can search for "RecyclerView partial refresh" to learn more about the specific differences.
Note that these methods refresh the UI, so if the list data hasn't changed, the refresh will have no effect.

```kotlin
rv.linear().setup {
    addType<SimpleModel>(R.layout.item_simple)
}.models = dataList

dataList.add(SimpleModel())

rv.bindingAdapter.notifyItemInserted(dataList.size) // Inserts a new item at the end
```
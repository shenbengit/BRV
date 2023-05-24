The data for the RecyclerView is a collection of `List<Any?>?`, where the collection elements are data models.

By having the data models implement different interfaces, you can obtain different functionalities or callbacks.

| Interface | Description |
|-|-|
| ItemBind | Callback in the data model's `onBindViewHolder` lifecycle, used for data binding. |
| ItemAttached | Listens to the view being attached to the window. |
| ItemExpand | Indicates that the item can be grouped. |
| ItemDrag | Indicates that the item can be dragged. |
| ItemSwipe | Indicates that the item can be swiped. |
| ItemHover | Indicates that the item can be hovered. |
| ItemPosition | Represents the index position of the item. |

Please refer to the corresponding feature sections or comments for specific usage.

```kotlin
data class SimpleModel(var name: String = "BRV") : ItemBind {

    override fun onBind(holder: BindingAdapter.BindingViewHolder) {
        // Use different methods to access the view components
        // holder.findView<TextView>(R.id.tv_simple).text = appName // Using findById
        // val binding = holder.getBinding<ItemMultiTypeOneBinding>() // Using DataBinding or ViewBinding
    }
}
```
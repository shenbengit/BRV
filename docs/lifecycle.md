## Lifecycle

First, let's start with some basic knowledge about RecyclerView (referred to as "rv" for short).

1. onCreateViewHolder (Create View): This callback is invoked to create the view. The number of times it is called is equal to the number of items that can be displayed on the screen simultaneously. If you need to frequently manipulate the view, it is recommended to do it in this callback.
2. onBindViewHolder (Bind Data): This callback is invoked every time an item is displayed on the screen. Therefore, it will be called repeatedly during fast scrolling of the list. It is recommended not to perform time-consuming operations inside this callback. For example, it is suggested to use [Serialize](https://github.com/liangjingkanji/Serialize) for efficient storage instead of SharedPreferences.

In BRV, these two functions are simplified as follows:

| Function | Description |
|-|-|
| onCreate | Corresponds to the `onCreateViewHolder` callback in the Adapter. |
| onBind | Corresponds to the `onBindViewHolder` callback in the Adapter. |
| onBindViewHolders | A collection of `onBindViewHolder` listeners, usually used by other frameworks for extension. Users generally do not need to use this directly. |

## Note

BindingAdapter is an `open class` and can be inherited and overridden. Any missing callback functions can be implemented by inheritance or anonymous class.

```kotlin
binding.rv.linear().adapter = object : BindingAdapter() {
    override fun onViewRecycled(holder: BindingViewHolder) {
        super.onViewRecycled(holder)
        // ....
    }
}.apply {
    addType<SimpleModel>(R.layout.item_simple)
    models = getData()
}
```

> Only the most recent `onBind` or `onCreate` callback is effective. There is a hierarchy of overrides.

```kotlin
rv.linear().setup {
    addType<SimpleModel>(R.layout.item_simple)
    onCreate {
        when(itemViewType){
            R.layout.item_simple -> {
                // Special handling
            }
        }
    }
}.models = getData()
```

## Show/Hide

By implementing the `ItemAttached` interface in the data model, you can be notified when an item is displayed or hidden. For example, item animations can be triggered when an item is displayed.

```kotlin
data class SimpleModel(var name: String = "BRV") : ItemAttached {

    var visibility: Boolean = false // Show/hide

    override fun onViewAttachedToWindow(holder: BindingAdapter.BindingViewHolder) {
        visibility = true
    }

    override fun onViewDetachedFromWindow(holder: BindingAdapter.BindingViewHolder) {
        visibility = false
    }
}
```

Alternatively, you can directly inherit from `BindingAdapter` to achieve the same result.
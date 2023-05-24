To achieve the sticky header effect, you can implement the `ItemHover` interface:

```kotlin
class HoverHeaderModel : ItemHover {
    override var itemHover: Boolean = true
}
```

By setting `itemHover` to `true`, the item will stick to the top.

Here's a complete example:

```kotlin
override fun onActivityCreated(savedInstanceState: Bundle?) {
    super.onActivityCreated(savedInstanceState)

    rv.linear().setup {
        addType<Model>(R.layout.item_simple)
        addType<HoverHeaderModel>(R.layout.item_hover_header)
        models = getData()

        // Click event
        onClick(R.id.item) {
            when (itemViewType) {
                R.layout.item_hover_header -> toast("Sticky item")
                else -> toast("Regular item")
            }
        }

        // Optional: Sticky listener
        onHoverAttachListener = object : OnHoverAttachListener {
            // When attaching to the top, v represents the sticky itemView
            override fun attachHover(v: View) {
                ViewCompat.setElevation(v, 10F)
            }

            // When detaching from the top
            override fun detachHover(v: View) {
                ViewCompat.setElevation(v, 0F)
            }
        }

    }
}
```

> Unlike most sticky frameworks, BRV doesn't require special handling to support all click events.

[BindingAdapter]: Determine if the current position belongs to a sticky item

```kotlin
fun isHover(position: Int): Boolean
```

## Sticky Grid

Demo screenshot:

<img src="https://i.loli.net/2021/08/14/4CfDnegM2kOi8WK.png" width="250"/>

As shown in the image, the sticky item is twice as wide as regular items. To achieve this, you need to dynamically determine the `SpanSize` of the sticky item, so you can't directly use `grid(3)`. Instead, you need to create a `HoverGridLayoutManager` manually:

```kotlin
val layoutManager = HoverGridLayoutManager(requireContext(), 2)
layoutManager.spanSizeLookup = object : GridLayoutManager.SpanSizeLookup() {
    override fun getSpanSize(position: Int): Int {
        return if(rv.bindingAdapter.isHover(position)) 2 else 1 // Specific business logic should be determined by you
    }
}
rv.layoutManager = layoutManager
```
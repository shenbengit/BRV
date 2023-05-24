1. The `divider` functions in BRV are implemented by creating a `DefaultDecoration`. If you want to access more functionality, please refer to the public functions in that class.
2. If the built-in divider functionality doesn't meet your requirements, you can inherit from `RecyclerView.ItemDecoration` to implement your own divider.

## Combining Spacing

You can repeatedly call `.divider` to stack multiple divider and spacing settings:

```kotlin
binding.rv.grid(3).divider { // Horizontal spacing
    orientation = DividerOrientation.HORIZONTAL
    setDivider(10, true)
}.divider { // Vertical spacing
    orientation = DividerOrientation.VERTICAL
    setDivider(20, true)
    onEnabled { // Enable dividers only for specific item type
        itemViewType == R.layout.item_divider_vertical
    }
}.setup {
    addType<DividerModel>(R.layout.item_divider_vertical)
}.models = getData()
```

## Methods

| Function | Description |
|-|-|
| onEnabled | Specifies whether the divider is enabled for the current item |
| addType | Specifies that dividers should only be drawn for the added item types |
| setColor | Sets the color of the divider |
| setDrawable | Sets the drawable resource for the divider |
| setBackground | Sets the background color of the divider |
| setMargin | Sets the spacing for the divider |

## Computing Edges

There is a utility class available to calculate the edge position of an item within the list. This can be used to set spacing. The `Edge.computeEdge()` function returns an `Edge` object:

```kotlin
data class Edge(
    var left: Boolean = false,
    var top: Boolean = false,
    var right: Boolean = false,
    var bottom: Boolean = false
)
```

If `left` is `true`, it indicates that the specified position is at the left edge of the list. Similarly, `top` being `true` indicates the item is at the top edge, and so on. You can use the results of this calculation to set the spacing for the itemView.
To enable drag and drop functionality, implement the `ItemDrag` interface for your data model:

```kotlin
data class DragModel(override var itemOrientationDrag: Int = ItemOrientation.ALL) : ItemDrag
```

Note that if your data model is deserialized using Gson, it will remove all field initialization values. To ensure the value is always returned, you can override the accessor function:

```kotlin hl_lines="3"
class DragModel() : ItemDrag {
    override var itemOrientationDrag: Int = 0
        get() = ItemOrientation.ALL // Always return this value
}
```

## ItemOrientation

The `ItemOrientation` class includes configurable drag directions:

| Field        | Description          |
|--------------|----------------------|
| `ALL`        | All directions       |
| `VERTICAL`   | Vertical direction   |
| `HORIZONTAL` | Horizontal direction |
| `LEFT`       | Left direction       |
| `RIGHT`      | Right direction      |
| `UP`         | Up direction         |
| `DOWN`       | Down direction       |
| `NONE`       | Disabled             |

## Customization

If you want to extend `ItemTouchHelper` or listen to its events, you can assign a value to the `itemTouchHelper` variable in the `BindingAdapter`:

```kotlin
rv.linear().setup {
    addType<Model>(R.layout.item)

    itemTouchHelper = ItemTouchHelper(object : DefaultItemTouchCallback(this) {

        /**
         * Called when the drag action is completed and the finger is released.
         */
        open fun onDrag(
            source: BindingAdapter.BindingViewHolder,
            target: BindingAdapter.BindingViewHolder
        ) {
            // This is called after the drag and drop operation, you can synchronize with the server here
        }

    })

}.models = data
```

Note that `DefaultItemTouchCallback` is the default touch event handling in BRV. You can override it or directly use `ItemTouchHelper.Callback`.

## Click and Drag

Directly clicking on an item to start dragging can cause gesture conflicts because both scrolling the list and dragging for sorting require movement gestures. Therefore, it is recommended to initiate dragging from a small icon within the item (dragging the icon will scroll the list).

Here's an example code:

```kotlin
rv.linear().setup {
    addType<DragModel>(R.layout.item_drag)
    onBind {
        findView<View>(R.id.btnDrag).setOnTouchListener { _, event ->
            if (event.action == MotionEvent.ACTION_DOWN) { // Start dragging when the finger is pressed
                itemTouchHelper?.startDrag(this)
            }
            true
        }
    }
}.models = getData()
```

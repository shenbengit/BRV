To enable drag and swipe functionality, you can implement the `ItemSwipe` interface for your data model:

```kotlin
data class SwipeModel(override var itemOrientationSwipe: Int = ItemOrientation.ALL) : ItemSwipe
```

> Note that if your data model is deserialized using Gson, it will remove all the field initialization values.

To solve this, you can override the accessor function to always return the desired value:

```kotlin
class SwipeModel() : ItemDrag {
    override var itemOrientationSwipe: Int = 0
        get() = ItemOrientation.ALL // Always return this value
}
```

## ItemOrientation

This class represents the configurable drag directions.

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

## Custom Swipe

If you want to extend ItemTouchHelper, you can assign a value to the `itemTouchHelper` variable in the BindingAdapter:

```kotlin
rv.linear().setup {
  addType<SwipeModel>(R.layout.item)

  itemTouchHelper = ItemTouchHelper(object : DefaultItemTouchCallback(this) {
    // Override functions here
    override fun onSwiped(viewHolder: RecyclerView.ViewHolder, direction: Int) {
        super.onSwiped(viewHolder, direction)
        // Callback after swiping, you can synchronize with the server here

        Log.d("Position", "layoutPosition = ${viewHolder.layoutPosition}")
        Log.d("Data", "SwipeModel = ${(viewHolder as BindingAdapter.BindingViewHolder).getModel<SwipeModel>()}")
    }
  })

}.models = data
```

You can customize the view that will move during the swipe by adding the `swipe` tag to the view. This will allow you to show the view behind the background:

```xml
<RelativeLayout
        android:layout_width="wrap_content"
        android:layout_height="80dp"
        android:orientation="horizontal"
        android:tag="swipe"/>
```

## Swipe Buttons

If you want to implement swipe buttons similar to the ones in the QQ app, it is recommended to use a custom item view instead of implementing it within the list.

In this case, you can use a third-party library called [SwipeToActionLayout](https://github.com/st235/SwipeToActionLayout).

<img src="https://github.com/st235/SwipeToActionLayout/raw/master/images/showcase.gif" width="50%"/>

> Note that this interaction effect belongs to the official iOS effect, and it is not recommended to replicate it on Android.

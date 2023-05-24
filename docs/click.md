## Functions

| Function | Description |
|-|-|
| onFastClick | Add an event listener for the specified Id |
| onClick | Add a click event listener for the specified Id with debounce (prevents repeated clicks within a default interval of 500 milliseconds). You can also [set the interval](#_4) |
| onLongClick | Add a long click event listener for the specified Id |

> By using the control's ID in the item's layout file, you can set click or long click events for any view. The click event for an item is added to the root layout of the item by setting its ID.

## One-to-Many Click Events

This approach is useful when multiple IDs need to handle the same click logic.

```kotlin
rv.linear().setup {
    addType<SimpleModel>(R.layout.item_simple)

    onClick(R.id.item) {
        // Click event for the item, where the root layout of the item has the ID R.id.item
    }
}.models = getData()
```

The `onClick` parameter is of variable length. You can specify multiple IDs, and there is an override behavior. The same applies to `onFastClick` and `onLongClick`.

```kotlin
rv.linear().setup {
    addType<SimpleModel>(R.layout.item_simple)

    onLongClick(R.id.item) {

    }
    onClick(R.id.btn_submit) {
        // `it` refers to the ID you set
    }

    onClick(R.id.btn_submit) {
        // This will override the previous callback logic because the IDs are the same
    }
}.models = getData()
```

## One-to-One Click Events

When there is a one-to-one mapping between an ID and a click event callback, you can use the following simplified syntax:

```kotlin
rv.linear().setup {
    addType<SimpleModel>(R.layout.item_simple)

    R.id.tv_simple.onClick {
        toast("Click Text")
    }
    R.id.item.onLongClick {
        toast("Click Item")
    }
}.models = getData()
```

## Click Debouncing

> **Debouncing**: Responding only to the first click within a certain interval, preventing repeated response to click events caused by fast user clicks.

Enabling click debouncing in BRV is straightforward. Simply use the `onClick` function to set the event listener. `onFastClick` does not include click debouncing.

The following code can modify the debounce interval (default is 500 milliseconds):

=== "Global Setting"
```kotlin
BRV.clickThrottle = 1000 // in milliseconds
```

=== "Per-instance Setting"
```kotlin hl_lines="2"
binding.rv.linear().setup {
clickThrottle = 1000 // override the global setting

        addType<SimpleModel>(R.layout.item_simple)
        R.id.item.onClick {
            toast("Click Text")
        }
    }.models = getData()
    ```
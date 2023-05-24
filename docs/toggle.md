BRV provides a toggle event trigger and listener, which can be used to iterate through all the list items. You can update the data or views in the callback function.

The "toggle" can be understood as "iterating" through the list items.

This feature is commonly used to switch between different editing modes in a list.

<img src="https://i.loli.net/2021/08/14/BVjGH7CT9lZ8KXa.gif" width="250"/>

Here's an example:

```kotlin
override fun onActivityCreated(savedInstanceState: Bundle?) {
    super.onActivityCreated(savedInstanceState)

    rv.linear().setup {
        addType<CheckModel>(R.layout.item_check_mode)

        // Listen to toggle events
        onToggle { position, toggleMode, end ->
            if (end) {
                // Show or hide the edit menu
                ll_menu.visibility = if (toggleMode) View.VISIBLE else View.GONE
            }
        }
    }.models = getData()
}

fun onClick(v: View) {
    rv.bindingAdapter.toggle() // Trigger the toggle event on click
}
```

Available functions:

```kotlin
fun toggle()
// Trigger the toggle mode (invert the current state)

fun getToggleMode(): Boolean
// Get the current toggle mode

fun setToggleMode(toggleMode: Boolean)
// Set the toggle mode. If the provided mode is the same as the current mode, the callback won't be triggered.

fun onToggle(block: (position: Int, toggleMode: Boolean, end: Boolean) -> Unit)
// Listen to toggle events. In this event, you can modify data or views as needed.
// position: Index of the list item during the iteration
// toggleMode: The toggle mode (Boolean)
// end: Whether the iteration is completed for all items
```

## RecyclerView

Here are some commonly used calls for `BindingAdapter`:

| Function         | Description                                                                                                                                                                                                   |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `bindingAdapter` | Returns the object if the adapter is a `BindingAdapter`, otherwise throws an exception.                                                                                                                       |
| `models`         | Data model collection. No need to execute `notify*` functions, it automatically refreshes using `notifyDataChanged`.                                                                                          |
| `_data`          | Similar to `models`, but it doesn't automatically refresh using `notifyDataChanged`.                                                                                                                          |
| `mutable`        | Read-only data model collection that allows addition and removal. You need to manually refresh the list using `notify*` functions. If the actual data is not assigned, this function will throw an exception. |
| `addModels`      | Adds data and automatically refreshes the list.                                                                                                                                                               |

## Layout Managers

The framework also provides extension functions for quickly creating layout managers. Here are the examples shown above:

=== "LinearLayoutManager"
    ```kotlin hl_lines="1"
    rv.linear().setup {
        addType<SimpleModel>(R.layout.item_simple)
    }.models = getData()
    ```
=== "GridLayoutManager"
    ```kotlin hl_lines="1"
    rv.grid(3).setup {
        addType<SimpleModel>(R.layout.item_simple)
    }.models = getData()
    ```
=== "StaggeredLayoutManager"
    ```kotlin hl_lines="1"
    rv.staggered(3).setup {
        addType<SimpleModel>(R.layout.item_simple)
    }.models = getData()
    ```

Related Functions:

| Function    | Description                                                   |
|-------------|---------------------------------------------------------------|
| `linear`    | Creates a linear list using `LinearLayoutManager`.            |
| `grid`      | Creates a grid list using `GridLayoutManager`.                |
| `staggered` | Creates a staggered grid list using `StaggeredLayoutManager`. |

## Dividers

The framework provides an extension function for quickly setting dividers:

```kotlin hl_lines="1"
rv.linear().divider(R.drawable.divider_horizontal).setup {
    addType<DividerModel>(R.layout.item_divider_horizontal)
}.models = getData()
```

The extension function actually uses the `DefaultDecoration` to create the object.

## Dialogs

Quickly create a list in a dialog using the extension function:

```
Dialog(activity).setAdapter(bindingAdapter).show()
```

Function:
```kotlin
fun Dialog.brv(block: BindingAdapter.(RecyclerView) -> Unit): Dialog
```
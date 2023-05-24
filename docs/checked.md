<img src="https://i.loli.net/2021/08/14/MIe74pdKf5c1hTX.gif" width="250"/>

Editable/multi-select lists are common in development, and with just a few lines of code, BRV can implement a selection mode: [Demo](https://github.com/liangjingkanji/BRV/blob/master/sample/src/main/java/com/drake/brv/sample/ui/fragment/CheckModeFragment.kt)

## Multi-select List

1. Create the list:
    ```kotlin
    rv.linear().setup {
        addType<CheckModel>(R.layout.item_check_mode)
    }.models = getData
    ```

2. Add a field to the Model for storing the selection state:
    ```kotlin hl_lines="2"
    data class CheckModel(
        var checked: Boolean = false,
        var visibility: Boolean = false
    ) : BaseObservable() // BaseObservable is used for DataBinding

3. Listen for selection events:
    ```kotlin hl_lines="3"
    rv.linear().setup {
       addType<CheckModel>(R.layout.item_check_mode)
       onChecked { position, isChecked, isAllChecked ->
            val model = getModel<CheckModel>(position)
            model.checked = isChecked
            model.notifyChange() // Notify UI of data changes
       }
    }.models = getData
    ```

4. Trigger the selection event:
    ```kotlin hl_lines="11"
    rv.linear().setup {
       addType<CheckModel>(R.layout.item_check_mode)
       onChecked { position, isChecked, isAllChecked ->
            val model = getModel<CheckModel>(position)
            model.checked = isChecked
            model.notifyChange() // Notify UI of data changes
       }
    
       onClick(R.id.cb, R.id.item) {
            var checked = getModel<CheckModel>().checked
            setChecked(adapterPosition, !checked) // Trigger the selection event when clicking on a list item to select/deselect it
       }
    }.models = getData
    ```

<br>

## Default Selection

If you want to default select certain items, you should use the `setChecked` function instead of directly setting the `isChecked` property to true in the Model (this won't trigger the selection callback).

For example, in the Demo, there is a line of code that defaults to selecting the first item:

```kotlin
// Toggle selection mode
tv_manage.setOnClickListener {
    adapter.toggle()
    rv.bindingAdapter.setChecked(0, true) // Select the first item initially
}
```

## Data Changes

If the data positions change, such as with additions or deletions, use `BindingAdapter.checkedPosition.clear()` to clear the selected position collection (or perform necessary operations), otherwise data errors may occur, leading to the inability to single-select.

## Functions

Common functions for selection mode support:

| Function | Description |
|-|-|
| allChecked | Select or deselect all items |
| singleMode | Check if it is in single-selection mode |
| isCheckedAll | Check if all items are selected |
| checkedReverse | Reverse the selection |
| setChecked | Set whether the item at the specified position is selected |
| checkedSwitch | Toggle the selection state |
| setCheckableType | Specify the type(s) that can be selected |
| getCheckedModels | Get the collection of selected data models |
| checkedPosition | Collection of positions of selected items |
| checkedCount | Number of items selected |
| onChecked | Selection callback |


BRV provides several features for handling expandable groups in a RecyclerView. These features include expand/collapse functionality, animations, recursive expansion/collapse, expanding groups to the top, ensuring only one group is expanded at a time, finding the parent group of an item, and supporting multiple group types.

To use these features, you need to implement the `ItemExpand` interface in your model class:

```kotlin
class GroupModel : ItemExpand {
    override var itemGroupPosition: Int = 0
    override var itemExpand: Boolean = false
    override var itemSublist: List<Any?>? = listOf(Model(), Model(), Model(), Model())
}
```

The `itemGroupPosition` represents the position of the item within its group, `itemExpand` indicates whether the item is expanded or collapsed, and `itemSublist` stores the sublist of the group.

To create the expandable group list, you can use the following code:

```kotlin
rv.linear().setup {
    addType<GroupModel>(R.layout.item_group_title)

    R.id.item.onClick {
        expandOrCollapse()
    }
}.models = getData()
```

In this example, the `addType` function is used to specify the type of the group item and its corresponding layout resource. The `R.id.item.onClick` block sets up a click listener for the item to expand or collapse it. The `getData()` function provides the list of data models.

To handle nested groups, you can customize the behavior of the item touch helper for drag and swipe actions. For example:

```kotlin
binding.rv.linear().setup {
    itemTouchHelper = ItemTouchHelper(object : DefaultItemTouchCallback() {
        override fun onSelectedChanged(viewHolder: RecyclerView.ViewHolder?, actionState: Int) {
            if (actionState == ItemTouchHelper.ACTION_STATE_DRAG) {
                (viewHolder as BindingAdapter.BindingViewHolder).collapse()
            }
            super.onSelectedChanged(viewHolder, actionState)
        }

        override fun onSwiped(viewHolder: RecyclerView.ViewHolder, direction: Int) {
            (viewHolder as BindingAdapter.BindingViewHolder).collapse()
            super.onSwiped(viewHolder, direction)

            val parentViewHolder = vh.findParentViewHolder()
            val parentGroup = parentViewHolder?.getModelOrNull<ItemExpand>()
            (parentGroup?.itemSublist as? ArrayList)?.remove(vh.getModelOrNull())
        }
    })

    // ...
}.models = getData()
```

This code demonstrates how to collapse the sublist of a group before dragging or swiping actions to avoid data inconsistency. The `findParentViewHolder()` function is used to find the parent group's ViewHolder, and the sublist is removed from the parent group's `itemSublist` if a nested item is swiped.

You can expand or collapse all groups by iterating over the data and setting the `itemExpand` property:

```kotlin
binding.rv.bindingAdapter.models = getData().forEach {
    it.itemExpand = true // or false to collapse all
}
```

The `ItemDepth` interface and `ItemDepth.refreshItemDepth` function can be used to calculate the depth of items in the list. This can be helpful for managing hierarchical groups.

For more detailed examples and explanations of the features, you can refer to the provided code snippets and the official documentation of the BRV library.
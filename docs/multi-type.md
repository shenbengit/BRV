In BRV, creating multiple types is very simple. You can add multiple types by calling `addType()` multiple times.

## Adding Multiple Types

### Many-to-Many

Each data type in the list corresponds to an item type.

```kotlin
rv.linear().setup {

    addType<Model>(R.layout.item_1)
    addType<Store>(R.layout.item_2)

}.models = data
```

### One-to-Many

All data types in the list are the same, but different item types are added based on a specific field in the data class.

```kotlin
rv.linear().setup {

    addType<Model>{
        // Use age to determine different layouts
        when (age) {
            23 -> {
                R.layout.item_1
            }
            else -> {
                R.layout.item_2
            }
        }
    }

}.models = data
```

The lambda inside the `addType` represents the specified generic type, so we can directly use `Model.age` to determine different types.

### Interface Implementation

You can add a type for an interface and then add multiple subclasses of that interface as data models. Each subclass can implement the interface differently.

Example:

```kotlin
interface BaseInterfaceModel {
    var text: String
}

data class InterfaceModel1(override var text: String) : BaseInterfaceModel

data class InterfaceModel2(val otherData: Int, override var text: String) : BaseInterfaceModel

data class InterfaceModel3(val otherText: String) : BaseInterfaceModel {
    override var text: String = otherText
}
```

Create sample data:

```kotlin
private fun getData(): List<Any> {
    return List(3) { InterfaceModel1("item $it") } +
            List(3) { InterfaceModel2(it, "item ${3 + it}") } +
            List(3) { InterfaceModel3("item ${6 + it}") }
}
```

Declare the list:

```kotlin
binding.rv.linear().setup {
    addType<BaseInterfaceModel>(R.layout.item_interface_type)
    R.id.item.onClick {
        toast("Click on the text")
    }
}.models = getData()
```

This is just a simple example with text. You can implement more complex business logic based on your requirements.

## Differentiating Types

As mentioned in the previous sections, each item type has its corresponding data type. Therefore, when performing business logic on the data, we need to handle different types differently.

Differentiate types based on `itemViewType`:

```kotlin
rv.linear().setup {
    addType<SimpleModel>(R.layout.item_simple)
    addType<ItemSimpleBinding2>(R.layout.item_simple)
    onBind {
        when(itemViewType) {
            R.layout.item_simple -> {
                getBinding<ItemSimpleBinding>().tvName.text = "Text content"
            }
            R.layout.item_simple_2 -> {
                getBinding<ItemSimpleBinding2>().tvName.text = "Type 2 - Text content"
            }
        }
    }
}.models = getData()
```

Differentiate types based on `getBinding()`:

```kotlin
rv.linear().setup {
    addType<SimpleModel>(R.layout.item_simple)
    addType<ItemComplexBinding>(R.layout.item_simple)
    onBind {
        when (val viewBinding = getBinding<ViewBinding>()) {
            is ItemSimpleBinding -> {
                viewBinding.tvName.text = "Text content"
            }
            is ItemComplexBinding -> {
                viewBinding.tvName.text = "Type 2 - Text content"
            }
        }
    }
}.models = getData()
```

Differentiate types based on `getModel()`:

```kotlin
rv.linear().setup

{
    addType<SimpleModel>(R.layout.item_simple)
    addType<ItemComplexBinding>(R.layout.item_simple)
    onBind {
        when (val model = getModel<Any>()) {
            is ChatModel -> {
                model.input = "Message content"
                model.notifyChange()
            }
            is CommentModel -> {
                model.input = "Comment content"
                model.notifyChange()
            }
        }
    }
}.models = getData()
```
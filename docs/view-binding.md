Assuming you don't want to use DataBinding and prefer to use ViewBinding, you can refer to the following information.

In fact, the usage of ViewBinding for finding view objects is similar to DataBinding, as both involve calling `getBinding()`.

## Introduction

It is highly recommended to use DataBinding instead of ViewBinding for the following reasons:

1. MVVM with two-way data binding is currently one of the best architectural designs, and DataBinding is the optimal solution for implementing MVVM.
2. ViewBinding is just a small feature within DataBinding and does not generate classes for all layouts by default, resulting in less code.

## Usage

If you still want to use ViewBinding, you can refer to the following code.

### 1) Using ViewBinding in `onCreateViewHolder`

```kotlin
rv.linear().setup {
    addType<SimpleModel>(R.layout.item_simple)

    // Use in the onCreateViewHolder lifecycle
    onCreate {
        getBinding<ItemSimpleBinding>().tvName.text = "Text content"
    }

    // Or use in the onBindViewHolder lifecycle
    onBind {
        val binding = getBinding<ItemSimpleBinding>()
    }
}.models = getData()
```

### 2) Using ViewBinding in `ItemBind`

If you are using a data model that implements the `ItemBind` interface:

```kotlin
class SimpleModel(var name: String = "BRV") : ItemBind {

    override fun onBind(holder: BindingAdapter.BindingViewHolder) {
        val binding = getBinding<ItemSimpleBinding>()
        binding.tvName.text = "Text content"
    }
}
```

In both cases, you can call the `getBinding()` method.

## Multiple Types

If you have multiple item types, make sure to check the type first to avoid binding the wrong ViewBinding.

```kotlin
class SimpleModel(var name: String = "BRV") : ItemBind {

    override fun onBind(holder: BindingAdapter.BindingViewHolder) {

        // Approach 1: Check the itemViewType
        when (holder.itemViewType) {
            R.layout.item_simple -> {
                getBinding<ItemSimpleBinding>().tvName.text = "Text content"
            }
            R.layout.item_simple_2 -> {
                getBinding<ItemSimpleBinding2>().tvName.text = "Type 2 - Text content"
            }
        }

        // Approach 2: Check the ViewBinding
        when (val viewBinding = getBinding<ViewBinding>()) {
            is ItemSimpleBinding -> {
                viewBinding.tvName.text = "Text content"
            }
            is ItemComplexBinding -> {
                viewBinding.tvName.text = "Type 2 - Text content"
            }
        }
    }
}
```

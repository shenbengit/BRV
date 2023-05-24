BRV does not require creating an Adapter, so many people may not know how to reuse similar lists. In reality, it's even simpler.

First, familiarize yourself with the [three data binding methods](index.md) in BRV.

## Using DataBinding

If you're using DataBinding, it's even easier to achieve reuse. The business logic of DataBinding is contained in the Model, while the views are defined in XML files. If the UI or business logic is the same, you can register the same class and XML file.

```kotlin
// List 1
rv.linear().setup {
    addType<Model>(R.layout.item_simple)
}.models = getData()

// List 2
rv2.linear().setup {
    addType<Model>(R.layout.item_simple)
}.models = getData()
```

### Handling Data Model Differences

If the data models are different but the XML is the same, you can use the following two approaches:

1. Wrap multiple classes into a single class object as the list data (everything is an object).
   ```kotlin
   class ComposeModel(var model: Model, var model2: Model2)

   rv.linear().setup {
       addType<ComposeModel>(R.layout.item_simple)
   }.models = getData()
   ```
2. Have multiple classes implement a specified interface: [Interface Types](/multi-type/#_4)
   ```kotlin
   interface ModelImpl {
       var text: String // Expose a getter function
   }
   class Model(): ModelImpl {
       override var text: String = "Other text"
   }
   class Model2(): ModelImpl {
       override var text: String = "Other text"
   }

   rv.linear().setup {
       addType<ModelImpl>(R.layout.item_simple)
   }.models = getData()
   ```

## Implementing the ItemBind Interface

With this data binding method, both the business logic and UI are implemented in a single function. You can reuse the same data model collection. If the UI is the same but the data is different, refer to the previous method of wrapping data models.

```kotlin
class SimpleModel(var name: String = "BRV") : ItemBind {

    override fun onBind(holder: BindingAdapter.BindingViewHolder) {
        val appName = holder.context.getString(R.string.app_name)
        holder.findView<TextView>(R.id.tv_simple).text = appName + itemPosition
    }
}
```

```kotlin
// List 1
rv.linear().setup {
    addType<SimpleModel>(R.layout.item_simple)
}.models = getData()

// List 2
rv2.linear().setup {
    addType<SimpleModel>(R.layout.item_simple2)
}.models = getData()
```

## Using onBind

The following approach is not very convenient for reuse. It's primarily for quick and simple scenarios, and it's not recommended for complex logic. If you want to copy and paste, feel free to use it.

```kotlin
rv.linear().setup {
    addType<SimpleModel>(R.layout.item_simple)
    onBind {
        findView<TextView>(R.id.tv_simple).text = getModel<SimpleModel>().name
    }
}.models = getData()
```

## Extension Functions

You can encapsulate the complete code for building reusable lists as Kotlin extension functions.

```kotlin
/**
 * Usage
 */
viewBinding.rv1.setupNews { }.models = listOf<News1Module>()

val adapter = viewBinding.rv2.setupNews { }
adapter.models = listOf<News1Module>()

/**
 * Adapter
 */
fun <T : INews> RecyclerView.setupNews(onItemContainerClick: (T) -> Unit = {}): BindingAdapter =
    linear()
        .divider(R.drawable.item_news_rv_divider)
        .setup {
            addType<INews>(R.layout.item_standard_news)
            onBind {

            }
            R.id.v_container.onClick { onItemContainerClick.invoke(getModel<T>()) }
        }

/**
 * Define Interface
 */
interface INews {
    val title: String
    val image: String
    val content: String
    val url: String
    val date: String
    val time: String
}

/**
 * Entity Class 1
 */
data class News1Module(
    val news1Field: String,
    override val title: String,
    override val image: String,
    override val content: String,
    override val url: String,
    override val date: String,
    override val time: String
) : INews

/**
 * Entity Class 2
 */
data class News2Module(
    val news2Field: String,
    val news2Field1: String,
    val news2Field2: String,
    val news2Field3: String,
    val news2Field4: String,
    val news2Field5: String,
) : INews {
    override val title: String
        get() = news2Field
    override val image: String
        get() = news2Field1
    override val content: String
        get() = news2Field2
    override val url: String
        get() = news2Field3
    override val date: String
        get() = news2Field4
    override val time: String
        get() = news2Field5
}
```
To implement flexible layouts in BRV, you can add the Google open-source library [flexbox-layout](https://github.com/google/flexbox-layout).

Add the dependency to your project:

```groovy
dependencies {
    implementation 'com.google.android.flexbox:flexbox:3.0.0'
}
```

Then, create a list:

<img src="https://i.loli.net/2021/08/14/KYkHmyCrDogiLsS.png" width="250"/>

```kotlin
rv.layoutManager = FlexboxLayoutManager(activity)

rv.setup {
    addType<FlexTagModel>(R.layout.item_flex_tag)
}.models = getData()
```

In this example, `FlexboxLayoutManager` is set as the layout manager for the `RecyclerView`. You can then use the `setup` function to configure your list, specifying the type and layout resource for the items. Finally, assign the data to the `models` property to populate the list.
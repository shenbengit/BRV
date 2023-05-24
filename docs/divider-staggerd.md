Waterfall Flow Divider

<img src="https://i.loli.net/2021/08/14/gx8mLuCNOFzWfIj.png" width="250"/>

```kotlin
rv.staggered(3).divider(R.drawable.divider_horizontal).setup {
    addType<DividerModel>(R.layout.item_divider_horizontal)

    onBind {
        // Simulate dynamic height
        val layoutParams = itemView.layoutParams
        layoutParams.height = getModel<DividerModel>().height
        itemView.layoutParams = layoutParams
    }
}.models = getData()
```

## Edge Divider

You can control the visibility of edge dividers using two fields:

| Field | Description |
|-|-|
| [startVisible](api/-b-r-v/com.drake.brv/-default-decoration/index.html#-2091559976%2FProperties%2F-900954490) | Determines if top and bottom edge dividers are visible |
| [endVisible](api/-b-r-v/com.drake.brv/-default-decoration/index.html#-377591023%2FProperties%2F-900954490) | Determines if left and right edge dividers are visible |
| [includeVisible](api/-b-r-v/com.drake.brv/-default-decoration/index.html#1716094302%2FProperties%2F-900954490) | Determines if edge dividers around the layout are visible |
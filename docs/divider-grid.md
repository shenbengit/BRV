Support for GridLayoutManager with grid layout dividers.

## Horizontal Dividers

<img src="https://i.loli.net/2021/08/14/oyjdg42zDUbkFtu.png" width="250"/>

```kotlin
rv.grid(3).divider(R.drawable.divider_horizontal).setup {
    addType<DividerModel>(R.layout.item_divider_horizontal)
}.models = getData()
```

## Vertical Dividers

<img src="https://i.loli.net/2021/08/14/ChG9ZnNiJyasWFr.png" width="250"/>

```kotlin
rv.grid(3, RecyclerView.HORIZONTAL)
  .divider(R.drawable.divider_vertical, DividerOrientation.VERTICAL)
  .setup {
    addType<DividerModel>(R.layout.item_divider_vertical)
}.models = getData()
```

## Grid Dividers

<img src="https://i.loli.net/2021/08/14/NLAPphzIU6yvVnt.png" width="250"/>

```kotlin
rv.grid(3).divider {
    setDrawable(R.drawable.divider_horizontal)
    orientation = DividerOrientation.GRID
}.setup {
    addType<DividerModel>(R.layout.item_divider_horizontal)
}.models = getData()
```

## Edge Dividers

You can control whether edge dividers are visible using two fields:

| Field | Description |
|-|-|
| [startVisible](api/-b-r-v/com.drake.brv/-default-decoration/index.html#-2091559976%2FProperties%2F-900954490) | Whether to show top and bottom edge dividers |
| [endVisible](api/-b-r-v/com.drake.brv/-default-decoration/index.html#-377591023%2FProperties%2F-900954490) | Whether to show left and right edge dividers |
| [includeVisible](api/-b-r-v/com.drake.brv/-default-decoration/index.html#1716094302%2FProperties%2F-900954490) | Whether to show surrounding dividers |

### 1) Top and Bottom

<img src="https://i.loli.net/2021/08/14/JBjETuMoaORFWHK.png" width="250"/>

```kotlin hl_lines="4"
rv.grid(3).divider {
    setDrawable(R.drawable.divider_horizontal)
    orientation = DividerOrientation.GRID
    startVisible = true
}.setup {
    addType<DividerModel>(R.layout.item_divider_horizontal)
}.models = getData()
```

### 2) Left and Right

<img src="https://i.loli.net/2021/08/14/IcxHsWafFQXh4Eg.png" width="250"/>

```kotlin hl_lines="4"
rv.grid(3).divider {
    setDrawable(R.drawable.divider_horizontal)
    orientation = DividerOrientation.GRID
    endVisible = true
}.setup {
    addType<DividerModel>(R.layout.item_divider_horizontal)
}.models = getData()
```

### 3) All Edges

<img src="https://i.loli.net/2021/08/14/UmhH5BgFA3a1W2Q.png" width="250"/>

```kotlin hl_lines="4 5"
rv.grid(3).divider {
    setDrawable(R.drawable.divider_horizontal)
    orientation = DividerOrientation.GRID
    startVisible = true
    endVisible = true
}.setup {
    addType<DividerModel>(R.layout.item_divider_horizontal)
}.models = getData()
```

## Divider Spacing

By default, dividers are based on the spacing

set for the RecyclerView.

<img src="https://i.loli.net/2021/08/14/MSFzurOVYbK7EwU.png" width="250"/>

```kotlin
binding.rv.grid(3, orientation = RecyclerView.VERTICAL).divider {
    orientation = DividerOrientation.GRID
    setDivider(1, true)
    setMargin(16, 16, dp = true)
    setColor(Color.WHITE)
}.setup {
    addType<DividerModel>(R.layout.item_divider_vertical)
}.models = getData()
```

<br>
You can set spacing based on the item using the `baseItemStart/baseItemEnd` parameters.

```kotlin
binding.rv.grid(3, orientation = RecyclerView.VERTICAL).divider {
    orientation = DividerOrientation.GRID
    setDivider(1, true)
    setMargin(16, 16, dp = true, baseItemStart = true)
    setColor(Color.WHITE)
}.setup {
    addType<DividerModel>(R.layout.item_divider_vertical)
}.models = getData()
```

## Grid Sticky Equally Spaced Dividers

For this requirement, it is recommended to use a nested list to avoid issues with dividers. This approach is commonly used to achieve this effect.

```kotlin
binding.rv.linear().setup {
    onCreate {
        if (itemViewType == R.layout.item_simple_list) { // Build nested grid list
            findView<RecyclerView>(R.id.rv).divider { // Build spacing
                setDivider(20)
                includeVisible = true
                orientation = DividerOrientation.GRID
            }.grid(2).setup {
                addType<Model>(R.layout.item_group_none_margin)
            }
        }
    }
    onBind {
        if (itemViewType == R.layout.item_simple_list) { // Set data for nested grid list
            findView<RecyclerView>(R.id.rv).models =
                getModel<NestedGroupModel>().itemSublist
        }
    }
    addType<NestedGroupModel>(R.layout.item_simple_list)
    addType<HoverHeaderModel>(R.layout.item_hover_header)
}.models = getData()
```
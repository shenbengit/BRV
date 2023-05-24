If you prefer to directly create item layouts with XML to achieve the divider effect, using simple `layout_margin` for spacing, I recommend using `layout_margin`.

## Horizontal Divider

<img src="https://i.loli.net/2021/08/14/IoBfnz6ERXVHlq3.png" width="250" />

Create a `drawable` file to describe the divider, which can be reused:

```xml
<shape xmlns:android="http://schemas.android.com/apk/res/android">
    <solid android:color="@color/dividerDecoration" />
    <size android:height="5dp" />
</shape>
```

Create the list:

```kotlin
rv.linear().divider(R.drawable.divider_horizontal).setup {
    addType<DividerModel>(R.layout.item_divider)
}.models = getData()
```

<br>

## Vertical Divider

<img src="https://i.loli.net/2021/08/14/rAeDXkfV6HxJUym.png" width="250"/>

Create a drawable as the divider:

```xml
<shape xmlns:android="http://schemas.android.com/apk/res/android">
    <solid android:color="@color/dividerDecoration" />
    <size android:width="5dp" />
</shape>
```

Create the list:

```kotlin
rv.linear(RecyclerView.HORIZONTAL).divider(R.drawable.divider_vertical).setup {
    addType<DividerModel>(R.layout.item_divider_vertical)
}.models = getData()
```

- Here, a `Drawable` resource is used to quickly set the divider. The width and height of the Drawable determine the width and height of the divider.
- For horizontal dividers, the width value of the Drawable is ignored (the actual width is determined by the RecyclerView's width).
- For vertical dividers, the height value of the Drawable is ignored (the actual height of the divider is determined by the RecyclerView's height).

## Edge Dividers

| Field | Description |
|-|-|
| [startVisible](api/-b-r-v/com.drake.brv/-default-decoration/index.html#-2091559976%2FProperties%2F-900954490) | Indicates whether the start divider is visible |
| [endVisible](api/-b-r-v/com.drake.brv/-default-decoration/index.html#-377591023%2FProperties%2F-900954490) | Indicates whether the end divider is visible |
| [includeVisible](api/-b-r-v/com.drake.brv/-default-decoration/index.html#1716094302%2FProperties%2F-900954490) | Indicates whether both start and end dividers are visible |

<img src="https://i.loli.net/2021/08/14/iL5epWdOQKnwZAc.png" width="250"/>

You can control the visibility of the start and end dividers using the two fields:

```kotlin hl_lines="3 4"
rv.linear().divider {
    setDrawable(R.drawable.divider_horizontal)
    startVisible = true
    endVisible = true
}.setup {
    addType<DividerModel>(R.layout.item_divider_horizontal)
}.models = getData()
```

## Full Wrap Dividers

<img src="https://i.loli.net/2021/08/14/lGSOPdg5A8WInoL.png" width="250"/>

This type of divider is called a grid divider and requires the use of `DividerOrientation.GRID`, which is not supported by LinearLayoutManager.

There are two ways to achieve this:

1. Use a GridLayoutManager with a spanCount of 1.
2. Use separate `View`s on both sides of

the RecyclerView to draw the dividers.

The first method is recommended. Here's an example:

```kotlin
rv.grid().divider {
    setDrawable(R.drawable.divider_horizontal)
    orientation = DividerOrientation.GRID
    includeVisible = true
}.setup {
    addType<DividerModel>(R.layout.item_divider_vertical)
}.models = getData()
```

## Divider Spacing

There are two ways to add spacing to the dividers:

1. Directly configure a `drawable` with the desired margin. Here's an example of a horizontal divider with a 16dp margin:

    ```xml
    <inset xmlns:android="http://schemas.android.com/apk/res/android"
        android:insetLeft="16dp"
        android:insetRight="16dp">
        <shape>
            <solid android:color="@color/dividerDecoration" />
            <size android:height="5dp" />
        </shape>
    </inset>
    ```

2. Use `setMargin()`:

    ```kotlin
    binding.rv.linear().divider {
        setDivider(1, true)
        setMargin(16, 0, dp = true)
        setColor(Color.WHITE)
    }.setup {
        addType<DividerModel>(R.layout.item_divider_vertical)
    }
    ```
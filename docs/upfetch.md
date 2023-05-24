The section discusses the concept of loading more by pulling down, not by pulling up. A common use case is loading previous chat history in a chat interface.

For example, in a chat history screen, the latest messages are at the bottom, and pulling down the list triggers loading the next page. In BRV, achieving this behavior requires just one line of code, which is an extension of [SmartRefreshLayout](https://github.com/scwang90/SmartRefreshLayout).

<img src="https://i.loli.net/2021/08/14/J9ZEOlKGHsQygwV.gif" width="250"/>

> Loading more by pulling down is essentially achieved by reversing the RecyclerView and reversing the layout of the content view (which is now displayed normally).

```kotlin hl_lines="8"
override fun onActivityCreated(savedInstanceState: Bundle?) {
    super.onActivityCreated(savedInstanceState)

    rv.setup {
        addType<Model>(R.layout.item_simple)
    }

    page.upFetchEnabled = true

    page.onRefresh {
        // Simulating a network request that succeeds after 2 seconds
        postDelayed({
            val data = getData()
            addData(data) { index <= 2 }
        }, 1000)
    }.showLoading() // Show loading state (default page)
}
```
Except for the highlighted property, the rest of the code remains the same as usual.

Layout code:

```xml
<com.drake.brv.PageRefreshLayout
    android:id="@+id/page"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/rv"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layoutManager="androidx.recyclerview.widget.LinearLayoutManager"
        app:reverseLayout="true"
        app:stackFromEnd="true" />

    <!--stackFromEnd=true ensures that when using UpFetch, the data aligns to the bottom instead of the top if it doesn't fill the screen-->
    <!--reverseLayout=true reverses the order of the data in the RecyclerView-->

</com.drake.brv.PageRefreshLayout>
```

You can also use the `app:page_upFetchEnabled` attribute to configure the behavior.

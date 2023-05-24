List Animation is used to add animations to the appearance of list items.

## Animation Types
You can quickly set several built-in animation types provided by BRV using the following function:

```kotlin
fun setAnimation(animationType: AnimationType)
```

The available animation types are specified using the `AnimationType` enum:

| Enum | Animation Description |
|-|-|
| ALPHA | Fade-in animation |
| SCALE | Scale animation |
| SLIDE_BOTTOM | Slide-in from the bottom |
| SLIDE_LEFT | Slide-in from the left |
| SLIDE_RIGHT | Slide-in from the right |

## Custom List Animation

If the default animations are not sufficient for your needs, you can customize the desired animation effect by referring to the built-in subclasses of `ItemAnimation`.

```kotlin
fun setAnimation(itemAnimation: ItemAnimation)
```

Here is an example using the source code of `AlphaItemAnimation`:

```kotlin
class AlphaItemAnimation @JvmOverloads constructor(private val mFrom: Float = DEFAULT_ALPHA_FROM) : ItemAnimation {

    override fun onItemEnterAnimation(view: View) {
        ObjectAnimator.ofFloat(view, "alpha", mFrom, 1f).setDuration(300).start() // Fade-in animation
    }

    companion object {

        private val DEFAULT_ALPHA_FROM = 0f // Initial opacity
    }
}
```
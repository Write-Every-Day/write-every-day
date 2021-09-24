# Position CSS property
## 1. syntax
<pre>
<code>
position: static;
position: relative;
position: absolute;
position: fixed;
position: sticky;
</code>
</pre>

## 2.Value
### 2.1 static
Default vaule. The top, right, botton, left, and z-index properies are not working at all.

### 2.2 relative
position : relative is positioned relative to its normal position. Other content will not be adjusted to fit into any gap left by the element.

### 2.3 absolute
position : absolute; is positioned relative to the nearest positioned ancestor(has property that is not static). Absolute positioned elements are removed from the normal flow. 
(Other elements will be repositioned)

### 2.4 fixed
position: fixed; is positioned relative to the viewport, which means it always stays in the same place even if the page is scrolled.
(The element is removed from the normal document flow.)

### 2.5 sticky
offset relative to its nearest scrolling ancestor and containing block


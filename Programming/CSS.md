# ==Cascading Style Sheets==

&emsp;

All of the basic HTML tags are classified as two types namely #inline elements and #block elements.

Every element in html is laid in a block of area in the browser. This block called as the box model contains the html element itself and the padding, border and the margin corresponding to the element. 

A block element will have margin on its all four sides whereas an inline element will have margin only on its left and right sides. Important to remember this when working with divs for which the display property was changed to inline (divs are block by default). Changing the display property to #inline-block would add top and bottom margins to an inline element.

### Collapse:

There are two kinds of collapsing in CSS namely padding collapse and margin collapse.

* Padding can collapse in terms of inline elements when they are aligned vertically (because it doesn't have top and bottom margins). If a padding sits between two margins it wouldn't collapse.
* Margin collapsing is the most prevalent. If two elements with margins are placed close to each other they will share their margin. 
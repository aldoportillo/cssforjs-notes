# Positioned Layout

## Intro

When it comes to positioned layout. It is a layout mode that allows multiple elements to overlap.

When we declare positioned layout on a div we do so like:

```css

div{
    position: relative;
    position: absolute;
    position: fixed;
    position: sticky;
    position: static; /* Default */
}
```

## Relative Positioning

Very subtle positioning. It allows us to constrain certain children and gives us new css properties.

```css

div{
    top: 0px;    /* Offsets top   */
    right: 0px;  /* Offsets right */
    left: 0px;   /* Offsets left  */
    bottom: 0px; /* Offsets bottom */

}
```

## Absolute Positioning

Will remove element from the DOM's order and place it wherever we specify with the same top, left, right, bottom properties. The DOM will act as if it's not there and follow a normal flow.

Absolute positioned elements will always sit at top (z-axis) so as not to disturb the document object model.

### Centering Trick

Margin: auto; will affect the height of an absolute positioned element as well.

This could be great for a modal :)

```css
  div {
    position: absolute;
    /*replaced by inset*/
    /* top: 0px;
    left: 0px;
    right: 0px;
    bottom: 0px;
     */
    inset: 0px;
    width: 100px;
    height: 100px;
    margin: auto;
    background: deeppink;
  }
```

## Containing Blocks

Absolutely positioned elements can only be contained by a position parent.

```css

.block {
    padding: 16px;
    border: 2px solid silver;
  }

  .relative.block {
    position: relative;
    border-color: black;
  }

  .pink-box {
    position: absolute;
    top: 0px;
    right: 0px;
    background: deeppink;
    width: 50px;
    height: 50px;
  }

```

```html

<div class="block">
  <div class="relative block">
    <div class="block">
      <div class="block">
        <div class="pink-box"></div>
      </div>
    </div>
  </div>
</div>

```

In this example we can see that the pink-box cascades outwards until it finds a container which is our relative block. The absolute positioned pink-box also ignores padding because it is not part of the flow layout.

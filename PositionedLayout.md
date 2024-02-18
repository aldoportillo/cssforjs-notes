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

## Stacking Contexts

Rules of stacking:

1. If two items are in flow and they overlap, DOM order wins.
2. If those two items, contain text, the text will rest at the top.
3. If one item is position layout and the other one isn't, position layout wins.
4. If both items are position layout, DOM order wins.

### Z-Indexing

If we want to stack the elements in anyway outside the DOM order, we need to use z-index. Quick note: this is only if the elements are being stacked a certain way because of the DOM. This will also not work if the elements don't have stacking context.

### More ways to get stacking context

- Setting opacity to a value less than 1
- Setting position to fixed or sticky (No z-index needed for these values!)
- Applying a mix-blend-mode other than normal
- Adding a z-index to a child inside a display: flex or display: grid container
- Using transform, filter, clip-path, or perspective
- Explicitly creating a context with isolation: isolate (More on this soon!)

### The Cutting Crew way to create stacking context

```css

  .pricing {
    /*This will create stacking context for all the children below*/
    isolation: isolate;
  }
  .card {
    position: relative;
    z-index: 1;
  }
  .primary.card {
    z-index: 2;
    margin: -32px -16px;
  }

```

## Fixed Positioning

Fixed positioning is like absolute; however, it can only be contained by the viewport. This means they are immune to scrolling.

This means it's great for modals:

```html

<style>
  .modal {
    /*
      For this party trick, we need:
      - Position absolute or fixed
      - All sides set to '0'
      - explicit dimensions
      - auto margins
    */
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    width: 85%;
    height: 200px;
    margin: auto;
  }
</style>

<div>
  <div class="modal-backdrop"></div>
  <div class="modal">Hello World</div>
</div>
<p>Background text</p>

```

Now assuming we don't set a position, it will remain at its position in the flow layout.

### Helpful code snippet

We face some problems when we have parent or grandparent that uses transform, filter or will-change because it transforms the fixed position to absolute. Here is a code snippet to debug this:

```javacript

// Replace “.the-fixed-child” for a CSS selector
// that matches the fixed-position element:
const selector = '.the-fixed-child';

function findCulprits(elem) {
  if (!elem) {
    throw new Error(
      'Could not find element with that selector'
    );
  }

  let parent = elem.parentElement;

  while (parent) {
    const {
      transform,
      willChange,
      filter,
    } = getComputedStyle(parent);

    if (
      transform !== 'none' ||
      willChange === 'transform' ||
      filter !== 'none'
    ) {
      console.warn(
        '🚨 Found a culprit! 🚨\n',
        parent,
        { transform, willChange, filter }
      );
    }
    parent = parent.parentElement;
  }
}

findCulprits(document.querySelector(selector));

```

Once found you can use a portal.

## Overflow

When content inside a fixed height div is too much to be contained. It extends beyond its bounds by default. This behavior can be mdoified using the overflow property. When it comes to a scrollbar on MacBook its better to enable it to always show in setting since that automatic feature is only available with a track pad.

```css
.overflow-container{
  overflow: visible; /*Default*/
  overflow: scroll; 
/* overflow-x overflow-y*/
  overflow: auto;
  overflow: hidden; 
}
```

Overflow hidden can be used for many things such as adding decoration that exceeds box to not introduce scrollbar.

### Scroll Containers

Overflow hidden is a scroll container without the scroll bars. So when declaring a container with an overflow other than its default property. It becomes a scroll container. How can we solve this problem? 

```css 

.overflow-container{
  overflow: clip;
}

```

If we were to add overflow-x and add a border radius it would apply the clip to both axis.

Be very careful when using this. In different screen sizes where height might change. We will render content that's clipped unusable.

#### Work Around

```html

<style>
  html, body {
    height: 100%;
  }
  .outer-wrapper {
    overflow-x: hidden;
    min-height: 100%;
    /*
      Adding a border so you can see the
      size/shape of this container:
    */
    border: 2px dashed silver;
  }
  .wrapper {
    background: pink;
  }
</style>

<div class="outer-wrapper">
  <div class="wrapper">
    <div class="flourish one"></div>
    <div class="flourish two"></div>
  </div>
  <p>Hello world</p>
</div>

```

This is a great solution because scroll containers only scroll when there's an overflow. Since outer-wrapper doesn't have a constrained height, the inner content can keep growing as well.

### Horizontal Overflow

Since Images are inline elements, they will line wrap if there is no available space. This means the overflow property alone won't help us create a horizontal scroll bar for images.

We can use the white-space property. The white-space property lets us  tweak how inline/inline-block elements wrap.

```css
.image-wrapper{
  overflow: auto;/* allows the wrapper to fill the entire width*/
  white-space: nowrap;
}
```

### Overflow in Positioned Layout


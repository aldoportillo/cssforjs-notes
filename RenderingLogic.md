# Rendering Logic

## Flow Layout

### Intro

#### Block Elements

Block elements fill up all horizontal even if the content is smaller by default. We can set the width of the content as fit content but even then nothing can be inline with a block element.

#### Inline Elements

Inline elements allow other elements to be on the same line. They also get treated like typography, so if you ever have an \<img> and notice it has extra height. It is line-height. They are also sensitive to whitespace in HTML. There will be a horizontal gap between inline elements if there is whitespace between them in HTML. Inline element line wrap as well.

#### Inline-Block Elements

Inline Block elements can accept block properties but behave like inline elements, but they do not line wrap.

### Width Algorithms

When using percentage based widths, they are based on the parent's width. This means that this won't include the margin of the child and it will overflow. So by default, block elements use auto not 100%.

- min-content: specifying that we want our element to become as narrow as it can, based on the child contents.
- max-content: it never adds any line-breaks so it will be as long or as short as horizontal text.
- fit-content: It doesn't add line breaks until we span the width of parent container. It is exactly the same as setting min-width as min-content and max-width as max-content.

#### Max Width Wrapper Utility

This Max Width wrapper utility class will allow you to center a div and add space on each size on larger screens.

```css

.max-width-wrapper{
    max-width: 50vw;
    margin-left: auto;
    margin-right: auto;
    padding-left: 16px;
    padding-right: 16px;
  }

```

```html
<div class="max-width-wrapper">
  <div class="card">
  </div>
</div>

```

### Height Algorithms

When it comes to height, you really don't want to explicitly set it. But what about a page layout? What if the height of the content is not enough so the footer goes up?

```css

  html, body {
    height: 100%;
  }
  .wrapper {
    min-height: 100%;
    border: solid;
  }

```

```html

<main class="wrapper">
  <p>
    I fill the viewport now!
  </p>
</main>

```

## Margin Collapse

### Intro: Margin Collapse

When it comes to Margin, Margin is like personal space. 2 adjacent item's margins can overlap instead of stacking.

### Rules of Margin Collapse

1. Only Vertical Margins Collapse: except when we switch our writing modes but this is so niche, so more robust. Only block direction elements collapse
2. Margins only collapse in flow layout: you will get no margin collapse in flex or grid
3. Only adjacent elements collapse: If the elements are separated by a break or border the margins wont collapse
4. Bigger Number wins: If two vertically adjacent containers have different margins. The greater one wins.
5. Nesting Doesn't Prevent Collapsing: If there is margin on a child. That margin will be applied to parent.
6. Blocking by padding or border: Similar to rule number 3. There is an obstruction leading to no collapse.
7. Blocked by Gap: Similar to rule 5. If the child's height is not the parent's height. Then there will be a gap and no collapse.
8. Blocked by Scroll Container: Scroll container will cause no collapse
9. Margins can collapse in same direction. Look at rule 4 for behavior.
10. More than two margins can collapse
11. Margins can be added if one is negative: all negative margins collapse all positive margins collapse. Finally, the numbers are added.

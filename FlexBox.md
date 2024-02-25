# Flex Box

When dealing with flex box there are two sizes to worry about:

- minimum content size - The smallest an item can get without its contents overflowing. Once its small the container becomes a scroll container.
- hypothetical size - The size the item is expected to be when declared using height in column, width in row, or flex-basis for the same effect as height and width. Once the container becomes too small. The hypothetical size will decrease. It's just a suggestion.

## Properties

### Flex Grow

flex-grow is a property that only does something when items are above their hypothetical size. This property will force the item to take up all available space on the **primary axis**. (row if flex direction is row). These numbers are like ratios, so default is 0. The more we increase from 0-10 the more effect we see.

### Flex Shrink

flex-shrink is a property that only does something when items are between min size and hypothetical size. The value assigned is the rate at which the container shrinks compared to its siblings. The item also cannot shrink past its minimum content size. We can set flex-shrink: 0; to make sure that an element never shrinks.

### Flex Basis

flex-basis is like defining width or height. Width if row is the primary axis or height if column is the primary axis. Why bother using it? Well flex-basis will not allow an item to scale below its minimum content size; whereas, height or width will. Since the flex shorthand is very common. It is best practice to stick to flex-basis as opposed to using height or width.

### Flex

flex is a shorthand that combines flex-grow, flex-shrink and flex-basis.

flex: 1 will assign flex-grow: 1, but it will also set flex-basis: 0. It won't affect the default value for flex-shrink, which is 1. Since flex-basis is a synonym for width in a flex row, we're effectively shrinking each child to have a “hypothetical width” of 0px, and then distributing all of the space between each child.

Best way to think of it is that flex: 1; will distribute space evenly between every element in the primary axis.

#### Constraints

We can also implement constrains when utilizing the flex property. Such as min-width: and max-width:.

### Wrapping

```css

  main {
    display: flex;
    flex-wrap: wrap;
  }
  article {
    flex: 1 1 150px;
    max-width: 250px;
  }
  article img {
    width: 100%;
  }
```

When it comes to our items overflowing our stretch container and creating a scroll container as a result. We can add a flex-wrap for better UI. Imagine we have a card named article with an image. Above we set the image's width to be 100% of the parent to remove its native size. We add a flex-wrap declaration to our flex container as well; however, the last element still spans the full width. Well we can kinda fix this by adding a flex-basis to our article card as opposed to getting it to default to 0.

### Content vs. Items

So whats the deal with align-content? How does it defer from align-items.

The best way to see the difference is to experiment a bit. align-items aligns the content with respect to the container and align-content aligns them with respect to the largest item. (In row its the largest item's height. In Column, its the largest item's width)

### Groups and Gaps

For the longest time, when using flex box layout, I would create a lot of extra divs. This is because I didn't understand the box model and positioned layout properly. One of the main problems was that I had a flex container and within that container I had 3 items. One needed to be in the start and the last two at the end, so I would do something like:

```react

 import styled from 'styled-components';

function Header() {
  return (
    <Wrapper>
      <Logo>My Thing</Logo>
      <UselessDiv>
        <AuthButton>Log in</AuthButton>
        <AuthButton>Sign up</AuthButton>
      </UselessDiv>
    </Wrapper>
  );
}

const UselessDiv = styled.div``

const Wrapper = styled.header`
  display: flex;
  gap: 8px;
  justify-content: space-between;
`;

const Logo = styled.a`
  font-size: 1.5rem;
`;

const AuthButton = styled.button``

export default Header;

```

This makes me pollute my markup. Luckily we can use margin auto since auto is a greedy value. Also note that I added space between the flex items with gap. 

```react

import styled from 'styled-components';

function Header() {
  return (
    <Wrapper>
      <Logo>My Thing</Logo>
      <AuthButton>Log in</AuthButton>
      <AuthButton>Sign up</AuthButton>
    </Wrapper>
  );
}

const Wrapper = styled.header`
  display: flex;
  gap: 8px;
`;

const Logo = styled.a`
  font-size: 1.5rem;
  margin-right: auto;
`;

const AuthButton = styled.button``

export default Header;

```

### Ordering

We have seen flex-direction: column and reverse and although this does orders elements different on the page. The DOM for the screen readers stays the same.

We have other meachanisims:

#### Order

Similar to z-index. The less the number the first that it displays. order: 1; > order: 2;

#### wrap-reverse

Causes elements to wrap upwards rather than downwards

#### tab-index

For accessability. Setting this is similar to ordering; however, it selects which elements will be tabbed through first but also if we set it to -1 the the element would be un-tab-able.

### Flex box interactions

#### Positioned Flex Children

An element cannot participate in multiple layout modes. Setting an element within a flex container as fixed. Will take it out of the flow order. WHat if we use relative since relative positioning keeps elements in their dom order? Well we can do this in order to make that child element a container for positioned elements.

#### Margin Collapse

As we touched on in the margin collapse lesson. Flex items do not collapse margins.

#### Flex box and z-index

Flex-box supports z-index as well!

## Recipes

### Holy Grail Layout

```html

<style>
  .wrapper{
    display: flex;
    flex-direction: column;
    height: 100vh;
  }

  .middle{
    flex: 1;
    display: flex;
    justify-content: space-between;
  }

  .middle > nav{
    flex: 1;
  }

  .middle > aside{
    flex: 1;
  }

  .middle > main{
    flex: 2;
  }


  
</style>

<div class="wrapper">
  <header class="box">Header</header>
  <section class="middle">
    <nav class="box">Nav</nav>
    <main class="box">Main Content</main>
    <aside class="box">Ad unit</aside>
  </section>
  <footer class="box">Footer</footer>
</div>

```

### Sticky Sidebar

```html

<style>
  .wrapper {
    display: flex;
    
  }

  nav {
    position: sticky;
    top: 0;
    align-self: flex-start;
  }

  main {
    flex: 1;
  }
</style>

<section class="wrapper">
  <nav class="box">
    <h2>Navigation</h2>
    <ul>
      <li>Section One</li>
      <li>Section Two</li>
    </ul>
  </nav>
  <main class="box">
    <p>This container contains random stuff to increase its height.</p>
    <img src="https://courses.joshwcomeau.com/cfj-mats/cat-300px.jpg" />
    <p>Normally, a blog post would exist in this container.</p>
    <img src="https://courses.joshwcomeau.com/cfj-mats/dog-one-300px.jpg" />
    <p>This container contains random stuff to increase its height.</p>
    <img src="https://courses.joshwcomeau.com/cfj-mats/cat-two-300px.jpg" />
    <p>Normally, a blog post would exist in this container.</p>
    <img src="https://courses.joshwcomeau.com/cfj-mats/dog-two-300px.jpg" />
  </main>
</section>

```


# Modern Component Architecture

Problem: as the project gets larger. How can we be confident that a component's style won't be affected by a parent style?

Solution: Use style components to write styles associated with a component and avoid scoping issues.

## Styled Components

This converts to...

```javascript

const Button = styled.button /*This `` syntax is called a tagged template literal meaning styled.button is a function */
  display: flex;

  &:hover {
    color: red;
  }
`;

```

```css
 
.abc123 {
  /* Vendor prefixes for legacy browsers: */
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
}

/* Plucks out the `hover` pseudo-class:  */
.abc123:hover {
  color: red;
}

```

### The styled object

The styled object has methods for every HTML tag used within the body.

### Installation

```bash

npm i styled-components

```

### Global Styling in Styled Components

First define a global styles module using styled-components

```javascript
// GlobalStyles.js
import { createGlobalStyle } from 'styled-components';

const GlobalStyles = createGlobalStyle`
  *, *::before, *::after {
    box-sizing: border-box;
  }

  html {
    font-size: 1.125rem;
  }

  body {
    background-color: hsl(0deg 0% 95%);
  }
`;

export default GlobalStyles;
```

Finally Import them into your app

```js

// App.js
import GlobalStyles from '../GlobalStyles';

function App() {
  return (
    <Wrapper>
      <Router>
        {/* An entire app here! */}
      </Router>

      <GlobalStyles />
    </Wrapper>
  )
}

export default App;
```

### Dynamic Styles

Styles that change.

We can get a property from props without styling it inline by using interpolation functions.

```js

const Button = ({ color, onClick, children }) => {
  return (
    <Wrapper onClick={onClick} color={color}> //Need to pass prop
      {children}
    </Wrapper>
  );
}

const Wrapper = styled.button`
  color: ${props => props.color}; //Accept prop and do whatever JS you want like a truthy statement
  padding: 16px 24px;
`;

```

### Composition

We can have one component act like a base for another.

```js
import styled from 'styled-components';

export default function App() {
  return (
    <PrimaryButton>Button</PrimaryButton>
  );
}

const Base = styled.button`
  font-size: 21px;
`;

const PrimaryButton = styled(Base)`
  background: blue;
  color: white;
`;

```

### Dynamic Tags

What if we want a component to function as a button or link depending on wether we choose to declare it in a form or as a link with the same styles.

```js

import { Link } from 'react-router-dom';

function Button({ href, children }) {
  return (
    <Wrapper to={href} as={href ? Link : 'button'}>
      {children}
    </Wrapper>
  );
}

const Wrapper = styled.button`
  /* styles */
`;

render(<Button href="/">Hello</Button>);


```

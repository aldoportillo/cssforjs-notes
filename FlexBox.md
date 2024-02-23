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
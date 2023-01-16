# Flexbox and Grid

## Include borders in width
```
html {
    box-sizing: border-box;
}
*,*::before, *::after {
    box-sizing: inherit;
}
```
## Flexbox
- flex container and flex items
- in parent
    - display: flex
    - flex-direction: row/column
    - flex-wrap: wrap/no-wrap/wrap-reverse
    - flex-flow: dir wrap
    - gap
    - justify-content
        - flex-start/end
        - center
        - space-around - space x on both sites of every box
            - on outer of outer elements x
            - betweenn elements 2x
        - space-between - space x only between elements
        - space-evenly - every gap x
    - align-items - in cross axis in a line
        - flex-start/end
        - center
        - baseline - to text
        - stretch - fill line
    - align-content - in cross axis about all lines (row distribution)
        - flex-start/end
        - center
        - space-around/between/evenly
        - stretch
- in children
    - order: number
    - flex-basis: %
    - flex-shrink/grow
    - align-self
- don't use width in flexbox - better flex-basis


## Grid
- parent
    - display: grid
    - gap
    - grid-template-columns: repeat(4, 1fr)
    - grid-template-rows: repeat(3, auto)
    - grid-template-areas
- in child
    - grid-column: 1/2
    - grid-row: span 3


## Cool stuff
- text overlay effect (on img)
    - in parent position: relative
    - in children position: absolute, bottom: , top: 
- object-position: 0 -300px
- object-fit: fill/contain/cover/scale-down/none
- shape-outisde effect
    - Firefox dev tools for shape-outisde
    - display: flow-root
    - float: left
- list-style-type: none
- background-color: transparent
- link in buttons with display: block
- when 2 margins meet they collapse into 1
- responsive images
    - width: 100%
    - picture element
        ```
        <picture>
            <source src="" media="">
            <img>
        </picture>
        ```
        - it loads first if media
    - img with srcset and size
    - background images in media query
    - https://responsivebreakpoints.com/
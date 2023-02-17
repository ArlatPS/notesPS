# Sass Introduction Notes

## Intro
- .sass and .scss both work, but scss way more popular and cooler
- declaration block {} with declarations comprised of properties and values
- in Sass styles can be placesd in declaration block of parent element
- any descendant - just nested
- direct descendant > .class
- **parent selector & - ex &:hover {}**
- chained classes .btn.btn-primary (without spaces)
- tool for CSS testing Percy.io
- three computations of CSS:
    - composite - blend elements together - cheap
    - paint
    - layout - very expensive
- files starting with _ - partials (not compiled on their own)
- @import "_partial"

## Variables
- global variable declaration
    - $error_color: #f00 !default
    - !default is optional to set this value as default so it can be overrun
- local variable
    ```
    sth {  
        $text_color: #ddd;  
        color: $text_color;  
    }
    ```
    - local to declaration block
- simple math can be used with variables
- variable types:
    - numbers
    - colors
    - strings
    - booleans
    - lists - like arrays
    - maps - like objects
- inline comments "\\" are deleted during compilation
- **string interpolation #{$var}**

## Mixins
```
@mixin name($arg) {  
    styles with $arg  
}  
.sth {  
    @include name(arg)  
}
```
- arguments can be passed in order or with keys
- default value in mixin declaration $arg: val
- if null as an argument then rule is skipped
- **a declaration block can be passed into a mixin**
```
@mixin foo() {  
    .inner {  
        @content  
    }  
}  
@include foo() {  
    color: red  
}
```  
- @extend
    - @extend .class - but it is way better to use placeholder
    - placeholder
        - %name
        - @extend %name

## Functions
- built in functions:
    - rgb, rgba, mix
    - adjust-hue
    - darken / lighten
    - saturate / desaturate
    - str-slice
    ```
    @function name($x) {  
        @return $x + 2;
    }
    ```
- @if and @else
- and and or

## Data Structures
- list
    - $list: 0 0 2px #fff
    - separated by spaces or comas (if you use both => nested arrays)
    - @each $i in $list
    - nth($list, 3) - indexed from 1
- map
    ```
    $mymap {  
        key: value,  
    } 
    ```
    - map-get($mymap, key)
    - @each can be used at it returns arrays [key, value]

## Architecture
- BEM
    - B - block - standalone entity ex. Component
    - E - element - part of block
    - M - modifier - a flag on a block or element, used to change apperance or behavior
- B_E-M
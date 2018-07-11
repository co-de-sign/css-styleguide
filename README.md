# CSS Styleguide

## Class naming convention

For most of our overall CSS architecutre, and specially for our class naming convention, we'll be using [RSCSS](http://rscss.io) as a reference and inspiration.

In simple terms, we'll have classes for components and for elements inside the components. Components will be named with at least two words, separated by a dash, and elements shall have no dashes, only lowercase letters.

**Right**

```scss
.menu-nav {
  > .item {}
}
```

**Wrong**

```scss
.menu_nav {
  > .menu_nav-item {}
}
```

We strongly recommend reading the full [RSCSS](http://rscss.io) documentation to better understand the whole CSS structure system in which we based ours.

## Coding style

Prefer hexadecimal color codes `#f7931e` with lowercase letters and abbreviate them to three characters when possible `#f36`.

### Color variables

We recommend using variables to set project's color palette with names beginning with `color-`. So when using it in any part of project it's implicit that is a color variable and we can take advantage of text auto complete listing only color variables.

When defining the color variables for a project, you can approach it in two different ways.

The first way is to set variables for every hexadecimal that will be used in the project. Some of these colors will be shades of another color, so you can name them with the suffix `-light`, `-dark`, `xlight`, `x-dark`, `xx-dark`, and so on, like this:

```scss
$color-action: #2aaafe;
$color-action-hover: #018fec;

$color-error: #de4163;

$color-coolgray: #c4ced9;
$color-coolgray-light: #f0f2f5;
$color-coolgray-xlight: #f8f9fa;
$color-coolgray-dark: #8d969b;
$color-coolgray-xdark: #51565b;
```

> [Here](http://codepen.io/caepenna/pen/qqBYRN) is an example of this applied in a component.

The prefered way to do this, however, is to set only the base colors. And then, when using them, use the function `mix` to mix the base color with either black (`#000`) or white (`#fff`) the amount desired.

```scss
$color-action: #2aaafe;
$color-error: #de4163;
$color-coolgray: #c4ced9;

.button {
  background-color: $color-action;
  &:hover {
    background-color: mix(#000, $color-action, 10%);
  }
}
```

> [Here](http://codepen.io/caepenna/pen/jVOxvo) is an example of this applied in a component.

This way you won't need multiple variables and also will have flexibility to chose shades of colors for your interface as desired when designing the UI. It is recommended to limit the number of shades used to simplify the interface visually, but that limitation can vary a lot from project to project.

Also, when using color variables, create them for colors and then create a different variable with a name that specify its use, the following way.

```scss
$color-blue: #2aaafe;
$color-pink: #de4163;
$color-coolgray: #c4ced9;

$color-action: $color-blue;
$color-error: $color-pink;
```

### Grays

When using shades of gray on your project, set the values using the HSL format (e.g.: `hsl(0, 0%, 30%)`).

HSL stands for hue, saturation, and lightness - and represents a cylindrical-coordinate representation of colors.

An HSL color value is specified with: `hsl(hue, saturation, lightness)`. This format makes it easy to see how dark or light the shade of gray is, and easy to change it by only changing the `lightness` value.

**Right**

```scss
color: hsl(0, 0%, 25%); // dark gray
color: hsl(0, 0%, 40%); // medium gray
color: hsl(0, 0%, 80%); // light gray
```

**Wrong**

```scss
color: #404040; // dark gray
color: $color-gray-40; // medium gray
color: #ccc; // light gray
```

It is also important to avoid many different shades of gray in the project. It is not common for a project to have more than 5 different grays, and in such case the project should be optimized to use only the grays necessary in order to improve the overall UX.

In order to a better management of the amount of grays and the use of them, we suggest you start the css file for colors with the color palette documented right away.

```scss
// light gray for background: hsl(0, 0%, 90%)
// light gray for font: hsl(0, 0%, 70%)
// dark gray for background: hsl(0, 0%, 40%)
// dark gray for font: hsl(0, 0%, 30%)
```

### Color Names

It should never be used in a project. Use it only for fast prototyping and testing specific components.

```scss
color: red;
color: gold;
color: fuchsia;
```

## Css for JavaScript

### Controlling DOM elements with Javascript

Don't use CSS classes to query DOM elements on JavaScript.

Set `role` attribute to your components and leave CSS classes for styling purposes only because it isn't obvious which class names are for styles and which have JS behaviors bound to them.

_Need to include a study about perfomance when querying through role attribute._

_Need to include a reference on not using classes to query DOM elements._

**Wrong**

```html
<div class='user-info'>...</div>
```

```javascript
$('.user-info').on('hover', function() { ... });
```

**Right**

```html
<div class='user-info' role='avatar-popup'>...</div>
```

```javascript
$('[role~="avatar-popup"]').on('hover', function() { ... });
```

### Add or remove classes for styling components

Use `role` attributes only to query elements. If your Javascript needs to change a style add/remove classes. Don't set/unset other element attributes.

## Designing for different sized plataforms

Responsive web design is not only the adaptation of the user experience to mobile devices, but designing and developing for every possible resolution and ratio of screen.

Therefore, one very useful strategy to responsive design and development is use a fluid structure to your layout. Container dimensions with values set in percentage or relative units (`vw`, `vh`) are often useful to make sure the sizes will adapt according to available space.

However, breakpoints are useful for adapting specificities and they should be used to better adapt the layout in specific circumstances. Also, break points should be set in variables so it is easier to change them when needed.

> The example below is based on [Foundation](http://foundation.zurb.com/sites/docs/media-queries.html), adapted to better suit our needs.

**Right**

```scss
// Defining values
$small-range:   (0em, 40em);       /* 0, 640px */
$medium-range:  (40.063em, 64em);  /* 641px, 1024px */
$large-range:   (64.063em, 90em);  /* 1025px, 1440px */
$xlarge-range:  (90.063em, 120em); /* 1441px, 1920px */
$xxlarge-range: (120.063em);       /* 1921px */

// Defining media queries
$small-only:   "only screen and (max-width: #{upper-bound($small-range)})" !default;
$medium-up:    "only screen and (min-width:#{lower-bound($medium-range)})" !default;
$medium-only:  "only screen and (min-width:#{lower-bound($medium-range)}) and (max-width:#{upper-bound($medium-range)})" !default;
$large-up:     "only screen and (min-width:#{lower-bound($large-range)})" !default;
$large-only:   "only screen and (min-width:#{lower-bound($large-range)}) and (max-width:#{upper-bound($large-range)})" !default;
$xlarge-up:    "only screen and (min-width:#{lower-bound($xlarge-range)})" !default;
$xlarge-only:  "only screen and (min-width:#{lower-bound($xlarge-range)}) and (max-width:#{upper-bound($xlarge-range)})" !default;
$xxlarge-up:   "only screen and (min-width:#{lower-bound($xxlarge-range)})" !default;

// Usage
@media #{$small-only}   { … }
@media #{$medium-up}    { … }
@media #{$medium-only}  { … }
@media #{$large-up}     { … }
@media #{$large-only}   { … }
@media #{$xlarge-up}    { … }
@media #{$xlarge-only}  { … }
@media #{$xxlarge-up}   { … }
```

Also, when setting dimensions for containers and main structural components, try using both fixed values and relative values so you can control how much it will vary according to the viewport, for example:

**Avoid**

```scss
.container {
  width: calc(50% + 20em);
  max-width: calc(100% - 2em);
}
```

**Prefer**

```scss
.container {
  width: 960px;
  max-width: 90%;
}
```

## Grid

## Horizontal grid

**Why columns? When to use?**

We believe in consistency, but also in a content-fluid decision. So that's why we built our own column grid system which should be used only in cases that you have to organize elements in a pre-defined horizontal system.

Not all layouts will have a column system, threfore the use of columns are not mandatory.

**Our Solution**

We aimed to create a framework that would work inside any container, regardless of its width. We just wanted to customize 3 variables: the width of the element container columns, the number of columns (`$nOfColumns`) and the gutter between columns (`$gutter`). Then the columns width would be adapted accordingly.

Our solution is a Sass based system that could be set in any layout/component.

It also includes custom settings for other sizes of screen you might be working with. It works by setting up a minimum screen size for tablet or mobile and later working out the columns, gutter variables for those screens.

**How to use?**

First things first: get to know your variables.

The code is really simple. And it will start as two variables only: number of columns and gutter.

```scss
$nOfColumns: 12;
$gutter: 1rem;
$columnWidth: ( 100%/#{$nOfColumns} - #{$gutter} );
```

You will notice that same variables applies for tablets and mobile.

```scss
$nOfColumnsTablet: 8;
$gutterTablet: .5rem;
$columnWidthTablet: ( 100%/#{$nOfColumnsTablet} - #{$gutterTablet} );
$nOfColumnsMobile: 4;
$gutterMobile: .5rem;
$columnWidthMobile: ( 100%/#{$nOfColumnsMobile} - #{$gutterMobile} );
```

And then, set up your maximum tablet and mobile width.

```scss
$tabletWidth: 640px;
$mobileWidth: 480px;
```

Second stuff comes right after: get to know the system.

There you'll see a bunch of _mixins_ for mobile and tablet. They will be setting up the grid the way it should be based on the variables you have set before. Apply it using `@include grid(number)`.


And also, there will be set some offset mixins in case you might wanna get off the grid for some reason. Apply it using `@include offSetRight(number)` or `@includeOffSetLeft(number)`.

Not all set, yet! Set up your container width too.

Mind that the grid works without setting a width, but it might be compromise the overall look of the application and it's certainly not the best approach for a multiple screen application.

```scss
.container {
  width: calc(60% + 240px);
  max-width: 100%;
  margin-right: auto;
  margin-left: auto;
  overflow: auto;
}
```

_Need to talk about offest_

All set! You're good to go!

## Vertical grid

In design, _vertical rhythm_ is the structure that guides a reader's eye through the content. Consistent vertical rhythm makes a layout more balanced and a more readable content.

In order to ensure a consistent vertical rhythm, we use a vertical grid module which is half the size of the `html`'s `font-size`, set in `em`. This means every* vertical distance should be a multiple of `0.5em`.

It is important to understand that not all layouts will have a consistent vertical rhythm. You could opt to not have this consistency for project-specific reasons and, in this case, this don't have to be taken into consideration.

**Project WITH consistent vertical rhythm**

```scss
html { font-size: 16px; }

p { line-height: 1rem; }

.foo { height: 2.5rem; }
```

**Project WITHOUT consistent vertical rhythm**

```scss
p { line-height: 17px; }

.foo { height: 2.4rem; }
```

**Furhter reading on this subject** [4 Simple Steps to Vertical Rhythm](http://typecast.com/blog/4-simple-steps-to-vertical-rhythm) [Aesthetic Sass 3: Typography and Vertical Rhythm](https://scotch.io/tutorials/aesthetic-sass-3-typography-and-vertical-rhythm) [CSS Baseline: The Good, The Bad And The Ugly](https://www.smashingmagazine.com/2012/12/css-baseline-the-good-the-bad-and-the-ugly/) [Using Sass & Compass Vertical Rhythm to set up typography defaults in a project.](https://medium.com/@amlinarev/using-sass-compass-vertical-rhythm-to-set-up-typography-defaults-in-a-project-34fe2f1d2c02#.4qrws1cww)

## Print

It's important for some pages to have a specific layout for print, especially pages with lots of text.

We recommend that print properties be declared in the same component file. But instead of component's properties to be in a separated media query:

**Right**

```scss
.example-component{
  width: 100%;
}

@media print {
  .example-component {
    width: 80%;
  }
}
```

**Wrong**

```scss
.example-component{
  width: 100%;
  @media print {
    width: 80%;
  }
}
```

Just like we follow RSCSS on CSS organization and class naming, we based our instructions in this post below.

We strongly recommend reading it: [I totally forgot about print style sheets](https://uxdesign.cc/i-totally-forgot-about-print-style-sheets-f1e6604cfd6#.702e2vrhq)

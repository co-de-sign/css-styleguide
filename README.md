# CSS Styleguide

This guide contains our best practices and suggestions that fit our work environment. Please be welcome to fork and/or make Pull Requests with suggestions, new topics, correcting us on typos, grammar, wrong assumptions, broken links, unclear text, etc. Pull Requests should contain a short description explaining what it is and its importancy.

[Contributing to the guide](https://github.com/co-de-sign/css-styleguide/blob/master/CONTRIBUTING.md)

## Contents
- [Class naming convention](#class-naming-convention)
- [Coding style](#coding-style)
- [Colors](#colors)
- [Css for JavaScript](#css-for-javascript)
- [Designing for different sized plataforms](#designing-for-different-sized-plataforms)
- [Grid](#grid)
- [Print](#print)
- [Stylesheets folders architecutre](#stylesheets-folders-architecutre)
- [Tools](#tools)
- [Typography](#typograph)
- [Units](#units)

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
[Codepen](https://codepen.io/caepenna/pen/kXqyyJ)

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

## Stylesheets folders architecutre

Codeign created the [Ambience-Base-Components Style Structure](https://github.com/co-de-sign/abcss) with the collaboration of the members, and after some iterations we come in a version that we believe is the best version to be in this styleguide

## Tools

### Sass
We recommend the use of SCSS syntax when using Sass.

**SCSS (Sassy CSS)**

```scss
h1 {font-size: $font-size-h1}

h2 {font-size: $font-size-h2}

.item {
  box-shadow: 0 2px 0px #dcffa6,
              0 2px 5px #000;
}
```

**Sass**

```sass
h1
  font-size: $font-size-h1

h2
  font-size: $font-size-h2

.item
  box-shadow: 0 2px 0px #dcffa6, 0 2px 5px #000
```
Comparing both syntaxes, SCSS allows you to write selectors and attributes on the same line so you can simplify your code. At the same time you need to be careful not to code in a way it's hard to read and understand.

You can write plain CSS in SCSS (or reuse CSS code from elsewhere). Also, for the same reason, the learning curve is smoother for people who already know CSS.

## Typography

In a project, the rules for typography should be pre-defined and systematic. By doing this it makes sure the overall look and feel is consistent and also prevents problems in the user experience.

### Font families

A project can have as many font families as you like, but each one of them must have a "reason to be". There should not be an extra font family for no reason, if there is no specific need or advantage for using it.

These font families should always be assigned to variables so they can be re-used in the project. The names of the variables should have a direct link with its "reason to be", as presented below.

**Right**

```scss
$font-family-body: "Tiempos Text", Georgia, Times, serif;
$font-family-headings: "Gotham", "Montserrat", Helvetica, serif;
$font-family-button: "FF DIN", "Roboto", sans-serif;
```

**Wrong**

```scss
$font-family-serif: "Tiempos Text", Georgia, Times, serif;
$font-family-gotham: "Gotham", "Montserrat", Helvetica, serif;
$button-font-family: "FF DIN", "Roboto", sans-serif;
```

Then, when assigning font families to selectors, you should assign it as few times as possible so the css code is easier to maintain.

**Right**

```scss
p {
  font-family: "Tiempos Text", Georgia, Times, serif;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  font-family: "Gotham", "Montserrat", Helvetica, serif;
}

.action-button {
  font-family: "FF DIN", "Roboto", sans-serif;
}
```

**Wrong**

```scss
.sidebar-component {
  > .description {
    font-family: "Tiempos Text", Georgia, Times, serif;
  }
}
```

### Font size & line height variables

For font size and line height values, the variables should be set using the metric prefixes. The variable names should refer to its size in relation to the "base size" (the size used in most of the body text in the project). This way we understand how big or small the text is directly from reading the variable name.

> Harry Roberts suggests the greek alphabet notation (`alpha`, `beta`, `gama`, etc) for setting font sizes names, but we prefere a way that makes more explicit the size in relation to each other (also, his example differs from our due to our use of mixins). You can find more interesting content on font sizing in CSS reading his article ["Pragmatic, practical font sizing in CSS"](http://csswizardry.com/2012/02/pragmatic-practical-font-sizing-in-css/).

**Right**

```scss
$font-size-mega: 4em;
$font-size-kilo: 3em;
$font-size-hecto: 2em;
$font-size-deca: 1.5em;
$font-size-base: 1em;
$font-size-deci: 0.75em;
$font-size-centi: 0.625em;

$line-height-mega: 3.5rem;
$line-height-kilo: 3rem;
$line-height-hecto: 2.5rem;
$line-height-deca: 2rem;
$line-height-base: 1.5rem;
$line-height-deci: 1rem;
$line-height-centi: .5rem;
```

**Wrong**

```scss
$font-size-extralarge: 4em;
$font-size-large: 3em;
$font-size-medium: 2em;
$font-size-small: 1em;
$font-size-extrasmall: 0.5em;
```

**Also Wrong**

```scss
$font-size-colossal: 4.5em;
$font-size-giant: 4em;
$font-size-huge: 3em;
$font-size-large: 2em;
$font-size-medium: 1em;
$font-size-small: 1em;
$font-size-tiny: .8em;
$font-size-minuscule: 0.6em;
```

Use the font size attribute on html tag selectors (h1, h2, p, etc) so you won't have to do so on specific situations unless it is an exception.

**Right**

```scss
p {
  font-size: $font-size-base;
  line-height: $line-height-base;
}

h1 {
  font-size: $font-size-mega;
  line-height: $line-height-mega;
}

h2 {
  font-size: $font-size-kilo;
  line-height: $line-height-kilo;
}

h3 {
  font-size: $font-size-hecto;
  line-height: $line-height-hecto;
}

h4 {
  font-size: $font-size-deca;
  line-height: $line-height-deca;
}

h5 {
  font-size: $font-size-centi;
  line-height: $line-height-centi;
}

h6 {
  font-size: $font-size-mili;
  line-height: $line-height-mili;
}

.news-item{
  > .caption {
    font-size: $font-size-centi;
    line-height: $line-height-centi;
  }
}
```

Avoid the need for repeating a font size variable in your code. Preferably, it should be set only twice: in the html tag selectors (e.g.: `h2`) or in a mixin (e.g.: `@mixin caption`).

**Right**

```scss
@mixin btn {
  font-size: $font-size-centi;
  // You would also want to add other "btn" styles here…
}

.action-button {
  @mixin btn;
}

.confirmation-popup {
  > .button {
    @mixin btn;
  }
}
```

**Wrong**

```scss
.action-button {
  font-size: $font-size-centi;
}

.confirmation-popup {
  > .button {
    font-size: $font-size-centi;
  }
}
```

### Font weight

The font weight value should be declared in numbers (e.g.: `100`), the only valid number ins CSS are multiples of 100, from 100 to 900.

**Right**

```scss
.title {
  font-weight: 700;
}
```

Each weight (hairline, thin, light, regular, medium, semibold, bold, heavy, black) should have its own number value (from 100 to 900). If there are any weights not mention before (such as “extra light”, “demibold” or “ultra black”) this framework can be adatped to fit them as log as thin is always 300, regular is always 400 and bold is always 700.

**Right**

```scss
@font-face {
  font-family: 'Greta Sans';
  src: url('fonts/GretaSans-.wotf');
  font-weight: 100;
}

@font-face {
  font-family: 'Greta Sans';
  src: url('fonts/GretaSans-.wotf');
  font-weight: 200;
}

@font-face {
  font-family: 'Greta Sans';
  src: url('fonts/GretaSans-Light.wotf');
  font-weight: 300;
}

@font-face {
  font-family: 'Greta Sans';
  src: url('fonts/GretaSans-Regular.wotf');
  font-weight: 400;
}

@font-face {
  font-family: 'Greta Sans';
  src: url('fonts/GretaSans-Medium.wotf');
  font-weight: 500;
}

@font-face {
  font-family: 'Greta Sans';
  src: url('fonts/GretaSans-Semibold.wotf');
  font-weight: 600;
}

@font-face {
  font-family: 'Greta Sans';
  src: url('fonts/GretaSans-Bold.wotf');
  font-weight: 700;
}

@font-face {
  font-family: 'Greta Sans';
  src: url('fonts/GretaSans-Heavy.wotf');
  font-weight: 800;
}

@font-face {
  font-family: 'Greta Sans';
  src: url('fonts/GretaSans-Black.wotf');
  font-weight: 900;
}
```
## Units

### Units for `font-size`

The `font-size` property should also have a value in either `em` or `rem`. The default unit in this case should be `em`, but if there's need to used the font size in `rem` it is fine :)

**Right**

```scss
h1 {font-size: 2em;}

h2 {font-size: 1.6em;}

p {font-size: 1em;}
```

**Wrong**

```scss
h1 {font-size: 32px;}

h2 {font-size: 10vw;}

p {font-size: 16pt;}
```

### Units for `line-height`

If the project have a consistent vertical rhythm, the `line-height` property should have a value multiple of the _vertical grid module_ of the vertical grid module in the project.

More information on [Vertical grid](#vertical-grid).

**Project WITH consistent vertical rhythm**

```scss
html {font-size: 16px;}

h1 {line-height: 2rem;}

h2 {line-height: 1.5rem;}

p {line-height: 1rem;}
```

**Project WITHOUT consistent vertical rhythm**

```scss
h1 {line-height: 34px;}

h2 {line-height: 26px;}

p {line-height: 16px;}
```

### Units for vertical margins and paddings

In general, vertical margins and paddings (top and bottom) are used as typographical tools to organize, group and arrange text. In order to maintain the vertical rhythm they should follow the same rules of the [Units for `line-height`](#units-for-line-height).

This means those values should always be multiple of `0.5rem` (if the project have a consistent vertical rhythm).

**Project WITH consistent vertical rhythm**

```scss
h3 {margin-bottom: 2rem;}
```

**Project WITHOUT consistent vertical rhythm**

```scss
h3 {margin-bottom: 32px;}

.sidebar > h3 {margin-bottom: 30px;}

.article > h3 {margin-bottom: 34px;}
```

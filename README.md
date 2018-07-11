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

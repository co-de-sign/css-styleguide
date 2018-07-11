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

**You should:**

- Use soft-tabs with a two space indent
- Put spaces before `{` in rule declarations (right: `.foo {}`; wrong: `.foo{}`)
- Use `//` for comment blocks (instead of `/* */`)

**Would be nice if you:**

- Avoid using `margin-top` and prefer the use of only `margin-bottom`. With all your elements having margin only below them it is easier to organize the overall structure.

## Order of properties declarations

In order for us to have a standard way of writing CSS we should always try to declare the properties in a specific order. This is not a "frozen" thing, and it is flexible, but the main guideline here is to group some properties together and have these blocks in a specific order.

The declarations groups are the following (and should follow this order):

1. **Mixins' includes**: these should be the first instructions to be declared. It is not a property, and it should be declared before so it can be overridden by properties declared below it.

2. **Box model properties**: The CSS box model is essentially a box that wraps around every HTML element. These are `width`, `height`, `padding`, `margin`, and `border`.

3. **Other layout/positioning properties**: properties with the function of positioning elements such as `display`, `position`, `top`, `right`, `bottom`, and `left`.

4. **Typography**: properties such as the `font` properties (i.e.: `font-size`, `font-family`, etc.), `text` properties (i.e.: `text-transform`, `text-decoration`, etc.) and `line-height`.

5. **Aesthetic/cosmetic properties**: properties that change the elements visual aspects without changing its size or positioning, such as `background`, `color`, `box-shadow`, `opacity`, `cursor`, etc.

6. **Transition**: `transition` properties (i.e.: `transition-delay`, `transition-duration`, `transition-property`, and `transition-timing-function`).

These groups should be separated by a blank line.

**Example**

```scss
.button {
  @include btn;

  width: 240px;
  margin-top: 2rem;
  margin-bottom: 4rem;

  position: absolute;
  top: 8rem;

  font-size: $font-size-body;
  text-transform: uppercase;
  text-decoration: underline;

  color: $color-highlight;
  background-color: $color-dark;
  opacity: 0.6;
  box-shadow: 0 2px 4px rgba(#000, 0.2);

  transition: opacity 0.2s ease-in;
}
```

Another relevant pattern to be followed is declaring abscissa properties before ordinate properties if both exists. For example, `transformX` should come before `transformY`; and `width` should come before `height`.

Properties should also be spread when possible to avoid conflicts of overriding when not wanted.

**Avoid**

```scss
.bg {
  background: fixed $color-main url(../assets/bg.png) center top no-repeat;
}
```

**Prefer**

```scss
.bg {
  background-attachment: fixed;
  background-color: $color-main;
  background-image: url(../assets/bg.png);
  background-position: center top;
  background-repeat: no-repeat;
}
```

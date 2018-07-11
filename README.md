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

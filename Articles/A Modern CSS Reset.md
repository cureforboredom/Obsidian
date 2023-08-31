---
date: 08-31-23
title: A Modern CSS Reset
summary: A very tiny css reset that makes life a lot easier.
length: 4min
tags: programming, code, css, modern
---

# A Modern CSS Reset
1st of October 2019

I think about and enjoy very boring CSS stuff—probably much more than I should do, to be honest. One thing that I’ve probably spent too much time thinking about over the years, is CSS resets.

In this modern era of web development, we don’t really need a heavy-handed reset, or even a reset at all, because CSS browser compatibility issues are much less likely than they were in the old IE 6 days. That era was when resets such as [normalize.css](https://github.com/necolas/normalize.css/) came about and saved us all heaps of hell. Those days are gone now and we can trust our browsers to behave more, so I think resets like that are probably mostly redundant.

## A reset of sensible defaults

I still like to reset stuff, so I’ve been slowly and continually tinkering with a reset myself over the years in an obsessive [code golf](https://en.wikipedia.org/wiki/Code_golf) manner. I’ll explain what’s in there and why, but before I do that, here it is in its entirety:

```css
/* Box sizing rules */
*,
*::before,
*::after {
  box-sizing: border-box;
}

/* Remove default margin */
body,
h1,
h2,
h3,
h4,
p,
figure,
blockquote,
dl,
dd {
  margin: 0;
}

/* Remove list styles on ul, ol elements with a list role, which suggests default styling will be removed */
ul[role='list'],
ol[role='list'] {
  list-style: none;
}

/* Set core root defaults */
html:focus-within {
  scroll-behavior: smooth;
}

/* Set core body defaults */
body {
  min-height: 100vh;
  text-rendering: optimizeSpeed;
  line-height: 1.5;
}

/* A elements that don't have a class get default styles */
a:not([class]) {
  text-decoration-skip-ink: auto;
}

/* Make images easier to work with */
img,
picture {
  max-width: 100%;
  display: block;
}

/* Inherit fonts for inputs and buttons */
input,
button,
textarea,
select {
  font: inherit;
}

/* Remove all animations, transitions and smooth scroll for people that prefer not to see them */
@media (prefers-reduced-motion: reduce) {
  html:focus-within {
   scroll-behavior: auto;
  }
  
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

## Breaking it down

We start with box-sizing. I just flat out reset all elements and pseudo-elements to use `box-sizing: border-box`.

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

Some people think that pseudo-elements should `inherit` box sizing, but I think that’s silly. If you want to use a [different box-sizing value](https://css-tricks.com/almanac/properties/b/box-sizing/), set it explicitly—at least that’s what I do, anyway.

```css
/* Remove default margin */
body,
h1,
h2,
h3,
h4,
p,
li,
figure,
figcaption,
blockquote,
dl,
dd {
  margin: 0;
}
```

After box-sizing, I do a blanket reset of `margin`, where it gets set by the browser styles. This is all pretty self-explanatory, so I won’t get into it too much.

```css
html:focus-within {
  scroll-behavior: smooth;
}
```

The “resetting” is now mostly done, so the first thing I do for core styles is set smooth scrolling. This was previously set right on the `html` element, but [recent updates](https://css-tricks.com/fixing-smooth-scrolling-with-find-on-page/) have resulted in this being updated to only apply smooth scrolling when there is `:focus-within` the `html` element.

```css
body {
  min-height: 100vh;
  text-rendering: optimizeSpeed;
  line-height: 1.5;
}
```

Next up: body styles. I keep this really simple. It’s useful for the `<body>` to fill the viewport, even when empty, so I do that by setting the `min-height` to `100vh`.

I only set two text styles. I set the `line-height` to be `1.5` because the default `1.2` just isn’t big enough to have accessible, readable text. I also set `text-rendering` to `optimizeSpeed`.

Using `optimizeLegibility` makes your text look nicer, but can have serious performance issues such as [30 second loading delays](https://marco.org/2012/11/15/text-rendering-optimize-legibility), so I try to avoid that now. I do sometimes add it to sections of microcopy though.

```css
ul[role='list'],
ol[role='list'] {
  list-style: none;
}
```

I only reset `list-style` where a list element has a `role=["list"]` attribute. This assists with some [accessibility issues, expertly explained by Scott](https://www.scottohara.me/blog/2019/01/12/lists-and-safari.html).

```css
a:not([class]) {
  text-decoration-skip-ink: auto;
}
```

For links without a class attribute, I set `text-decoration-skip-ink: auto` so that the underline renders in a much more readable fashion. This could be set on links globally, but it’s caused one or two conflicts in the past for me, so I keep it like this.

```css
img {
  max-width: 100%;
  display: block;
}
```

Good ol’ fluid image styles come next. I set images to be a block element because frankly, life is too short for that weird gap you get at the bottom, and realistically, images—especially with work I do—tend to behave like blocks.

```css
input,
button,
textarea,
select {
  font: inherit;
}
```

Another thing I’ve finally been brave enough to set as default is `font: inherit` on input elements, which as a shorthand, [does exactly what it says on the tin](https://css-tricks.com/almanac/properties/f/font/). No more tiny (mono, in some cases) text!

```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

Last, and by no means least, is a single `@media` query that resets animations, transitions and scroll behaviour if the [user prefers reduced motion](https://css-tricks.com/introduction-reduced-motion-media-query/). I like this in the reset, with specificity trumping `!important` selectors, because most likely now, if a user doesn’t want motion, they won’t get it, regardless of the CSS that follows this reset.
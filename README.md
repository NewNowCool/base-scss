# How To

Create a configuration file and include in your main scss file. See `/impl` for usage examples.

## Configuration file

The configuration file will setup the library with breakpoints and flex layout if `$bases` is set.

`lib/_nn.scss`

```
// $bases enables scaling layout
// use px value to stop scaling
// use a factor of 10 to workaround chrome issue
@forward "../../src" with (
  $reset: true, // include reset, default true

  $breakpoints: (
    tablet: 720px,
    desktop: 1024px,
    box: 1280px
  ),

  // breakpoints names and size values may be used as keys
  $bases: (
    // scaling
    0: 420,
    tablet: 768,
    desktop: 1024,

    // stop scaling
    box: 1px
  )
);
```

## Main SCSS

```
@use "lib/nn";
```

This will setup the breakpoints and flex layout if enabled in the configuration file.

## In Components

```
@use "../lib/nn" as *;
```

This will make mixins and placeholders available in the component.

# Usage

## Reset

We reset (not normalize) styles because we want full control and no default styles to interfere.

The default is to include the reset. You may disable by setting `$reset: false` in the configuration file.

## Flex Layout

The idea behind the flex layout is to have the page scale like an image. To achieve this, we use `vw` units for sizing.
By setting the root font size in `vw`, we can use `rem` units for everything else. The trick is to define the root font size that a pixel in the design matches 1rem in code.
Due to a chrome issue, we sadly have to use a factor of 10 (`1px = 0.1rem`) though. That way, we can easily translate pixel based designs into rem based code.

## Media Queries

Available media query mixins:

```
@include mq-from() {}
@include mq-to() {}
@include mq-between() {}
```

You can use breakpoint names or pixels.

```
@include mq-from(tablet) {}
@include mq-to(1000px) {}
@include mq-between(mobile, 2000px) {}
```

The media queries include the lower bound and exclude the upper bound.

```
@include mq-to(1000px) {} // 0px to 999px
@include mq-from(1000px) {} // 1000px and up
@include mq-between(1000px, 2000px) {} // 1000px to 1999px
```

## Easings

Available easing variables:

```
$linear: cubic-bezier(0.25, 0.25, 0.75, 0.75);
$ease: cubic-bezier(0.25, 0.1, 0.25, 1);
$easeIn: cubic-bezier(0.42, 0, 1, 1);
$easeOut: cubic-bezier(0, 0, 0.58, 1);
$easeInOut: cubic-bezier(0.42, 0, 0.58, 1);

$easeInQuad: cubic-bezier(0.55, 0.085, 0.68, 0.53);
$easeInCubic: cubic-bezier(0.55, 0.055, 0.675, 0.19);
$easeInQuart: cubic-bezier(0.895, 0.03, 0.685, 0.22);
$easeInQuint: cubic-bezier(0.755, 0.05, 0.855, 0.06);
$easeInSine: cubic-bezier(0.47, 0, 0.745, 0.715);
$easeInExpo: cubic-bezier(0.95, 0.05, 0.795, 0.035);
$easeInCirc: cubic-bezier(0.6, 0.04, 0.98, 0.335);
$easeInBack: cubic-bezier(0.6, -0.28, 0.735, 0.045);

$easeOutQuad: cubic-bezier(0.25, 0.46, 0.45, 0.94);
$easeOutCubic: cubic-bezier(0.215, 0.61, 0.355, 1);
$easeOutQuart: cubic-bezier(0.165, 0.84, 0.44, 1);
$easeOutQuint: cubic-bezier(0.23, 1, 0.32, 1);
$easeOutSine: cubic-bezier(0.39, 0.575, 0.565, 1);
$easeOutExpo: cubic-bezier(0.19, 1, 0.22, 1);
$easeOutCirc: cubic-bezier(0.075, 0.82, 0.165, 1);
$easeOutBack: cubic-bezier(0.175, 0.885, 0.32, 1.275);

$easeInOutQuad: cubic-bezier(0.455, 0.03, 0.515, 0.955);
$easeInOutCubic: cubic-bezier(0.645, 0.045, 0.355, 1);
$easeInOutQuart: cubic-bezier(0.77, 0, 0.175, 1);
$easeInOutQuint: cubic-bezier(0.86, 0, 0.07, 1);
$easeInOutSine: cubic-bezier(0.445, 0.05, 0.55, 0.95);
$easeInOutExpo: cubic-bezier(1, 0, 0, 1);
$easeInOutCirc: cubic-bezier(0.785, 0.135, 0.15, 0.86);
$easeInOutBack: cubic-bezier(0.68, -0.55, 0.265, 1.55);
```

## Fonts

Load fonts using the font-face mixin:

```
@include font-face(Roboto, "../../fonts/Roboto", normal, normal);
```

The mixin takes the following arguments:

```
mixin font-face(
  $name,
  $path,
  $weight: null,
  $style: null,
  $exts: eot woff2 woff ttf svg
)
```

# 🥒 Pickle

> Minimalist and opinionated Sass toolkit.

⚠️ Work in progress - Project just started

## Setup

**Simple**

```scss
@include pickle;
```

**Advanced**

```scss
/* default config */
$config: (

    // toggle wireframes
    $wires: true,

    // grid system
    $grid: (
        $gutter: 3,
        $direction: LTR,
        $max-width: 1440px
    ),

    // custom breakpoints
    $breakpoints: (
        'phone-only': (max-width: 599px),
        'tablet-portrait-up': (min-width: 600px),
        'tablet-up': (min-width: 900px),
        'desktop-up': (min-width: 1200px),
        'big-desktop-up': (min-width: 1800px)
    ),

    // colors
    $colors: (
        // 'beige': ($hex: '#f5f5dc')
    ),

    // custom fonts
    $fonts: (
        // 'Primary': (
        //     $folder: './fonts',
        //     $ext: 'ttf' 'woff2',
        //     $family: #{'Helvetica', Arial, sans-serif},
        //     $ratio: 1
        // )
    ),
    
    // custom sizes
    $sizes: (
        // 'XL': (
        //     $default: 1.6rem,
        //     $breakpoints: ('desktop-up': 2rem)
        // )
    ),

    // variables
    $vars: (
        // 'header-height': 4rem
    )

);

/* optional config */
@include pickle($config);
```

## Usage

```scss
/* breakpoints */
@include pickle-respond('tablet-up') {
    // ..
}

/* grid system */
.container { @include pickle-container(); }
.container .column { @include pickle-column(2/4); }
.container .span { @include pickle-span(1/3); }

/* typography */
@include pickle-typo($font: 'Primary', $size: 'XL');

/* colors */
@include pickle-color('beige');
@include pickle-bgcolor('beige');

/* global vars */
.classname { height: map-get($pickle-vars, 'header-height'); }
```

## FAQ

### Aren't breakpoints tied to specific devices bad practice?

According the [Sass guidelines](https://sass-guidelin.es/#responsive-web-design-and-breakpoints), media queries should not be tied to specific devices, and privilege names such as `medium` `large` `huge` rather than `tablet` `computer` `tv`.

**Pickle** takes a different stand on the question, as we believe ambiguity breeds confusion. A term like `medium` is too broad and can be interpreted in many different ways, while `tablet` is more clear and easy to remember. The idea is not about being 100% accurate across all devices, but to be as declarative as possible and easy to remember.

Not happy with the default naming changing convention? Just use your own by updating the default `$breakpoints` config ;)

### What's the correct way to manage typography styles?

Separation of concerns is a developement phylosophy that we truly believe in, and it also applies to typography. With **Pickle**, we decided to propose an approach that make it really easy to maintain shared typography settings such as font family, or size groups that respond to different breakpoints, while giving freedom and flexibility for more specific rules.

```scss
.title {
    @include pickle-typo($font: 'Primary', $size: 'XL');

    // custom rules
    @include pickle-color('beige');
    text-decoration: underline;
}

.subtitle {
    @include pickle-typo($font: 'Primary', $size: 'XS');
}
```

And because we know that all fonts are designed differently, we've made it even easier to manage sizing differences across different font families.

```scss
$config: (
    $fonts: (
        'Primary': ($ratio: 1, /* ... */)
        'Secondary': ($ratio: 1.2, /* ... */)
    ),
    $sizes: (
        'M': ($default: 1.6rem)
    )
)
```

Using the above config, `@include pickle-typo($font: 'Primary', $size: 'M');` output size is equal to `1.6rem`, while `$font: 'Secondary'` is equal to `1.92rem`.

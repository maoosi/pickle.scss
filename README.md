# 🥒 Pickle

> Minimalist and opinionated Sass toolkit.

⚠️ Work in progress - Project just started

## Features

- Consistant cross-browser default styling using [Sanitize.css](https://github.com/csstools/sanitize.css).
- Basic wires theme for quick UI prototyping (can be disabled).
- Smart grid system based on [Jeet](https://github.com/mojotech/jeet), a human-centered precision grid.
- Declaratives breakpoints system for handling media queries.
- Flexible typography setup with strong separation of concerns.

## Getting started

### Installation

```scss
@import '~pickle';
@include pickle;
```

### Advanced config

The below code shows the default config. All parameters are optional.

```scss
$config: (

    /* toggle wireframes */
    $wires: false,

    /* adjust grid system */
    $grid: (
        gutter: 3,
        direction: LTR,
        max-width: 1440px
    ),

    /* set custom breakpoints */
    $breakpoints: (
        'phone-only': (max-width: 599px),
        'tablet-portrait-up': (min-width: 600px),
        'tablet-up': (min-width: 900px),
        'desktop-up': (min-width: 1200px),
        'big-desktop-up': (min-width: 1800px)
    ),

    /* set project colors */
    $colors: (
        /* 'beige': #f5f5dc */
    ),

    /* declare project fonts */
    $fonts: (
        /* 'Primary': (
            folder: './fonts',
            ext: 'ttf' 'woff2',
            family: #{'Helvetica', Arial, sans-serif},
            ratio: 1
        ) */
    ),
    
    /* set custom size groups */ 
    $sizes: (
        /* 'XL': (
            default: 1.6rem,
            breakpoints: ('desktop-up': 2rem)
        ) */
    ),

    /* create global variables */
    $vars: (
        /* 'header-height': 4rem */
    )

);

/* init pickle with config */
@include pickle($config);
```

### Usage

```scss
/* breakpoints */
@include pickle-respond('tablet-up') {
    /* ... */
}

/* grid system */
.container { @include pickle-container(); }
.container .column { @include pickle-column(2/4); }

/* typography */
@include pickle-typo($font: 'Primary', $size: 'XL');

/* colors */
@include pickle-color('beige');
@include pickle-bgcolor('beige');

/* global vars */
.classname { height: map-get($pickle-vars, 'header-height'); }
```

## FAQ

### Aren't breakpoints tied to specific devices considered bad practice?

According the [Sass guidelines](https://sass-guidelin.es/#responsive-web-design-and-breakpoints), media queries should not be tied to specific devices, and privilege names such as `medium` `large` `huge` rather than `tablet` `computer` `tv`.

**Pickle** takes a different stand on the question, as we believe ambiguity breeds confusion. A term like `medium` is too broad and can be interpreted in many different ways, while `tablet` is more clear and easy to remember. The idea is not about being 100% accurate across all devices, but to be as declarative as possible and easy to use.

Not happy with the default naming convention? Just use your own by updating the default `$breakpoints` config ;)

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
        'Primary': (ratio: 1, /* ... */)
        'Secondary': (ratio: 1.2, /* ... */)
    ),
    $sizes: (
        'M': (default: 1.6rem)
    )
)
```

Using the above config, `@include pickle-typo($font: 'Primary', $size: 'M');` output size is equal to `1.6rem`, while `$font: 'Secondary'` is equal to `1.92rem`.

### How to use pickle grid system?

**Pickle** act as flavoured wrapper around the original [Jeet](https://github.com/mojotech/jeet) API, a human-centered precision grid. For more details on supported parameters for each of the below mixins, please refer to [Jeet API docs](https://github.com/mojotech/jeet/blob/master/docs/api.md).

```scss
/*  Container
    pickle-container(); */
@include pickle-container;

/*  Column with gutter
    pickle-column($ratios: 1, $offset: 0, $cycle: 0); */
@include pickle-column(2/4);

/*  Column without gutter
    pickle-span($ratio: 1, $offset: 0, $cycle: 0); */
@include pickle-span(2/4, $cycle: 4);

/*  Move columns/span, works similar to offset or push/pull
    pickle-move($ratios: 0, $col-or-span: column); */
@include pickle-move(-1/3);

/*  Disable source ordering setup previously
    pickle-unmove(); */
@include pickle-unmove;

/*  Center containers
    pickle-center($max-width: map-get($pickle-grid, 'max-width'), $pad: 0); */
@include pickle-center(420px);

/*  Reset styles associater with pickle-center()
    pickle-uncenter(); */
@include pickle-uncenter;

/*  Stack elements on top of each other
    pickle-stack($pad: 0, $align: false); */
@include pickle-stack;

/*  Cancel stack()
    pickle-unstack(); */
@include pickle-unstack;

/*  A simple/kinda-modern clearfix
    pickle-clearfix(); */
@include pickle-clearfix;
```

## Contributing

```bash
# install parcel globally
npm install -g parcel-bundler

# install dependencies
yarn install

# run the playground
yarn playground
```
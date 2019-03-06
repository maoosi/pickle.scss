# Pickle.scss

> 🥒 Minimalist and opinionated Sass toolkit.


## Features

- Consistant cross-browser default styling using [Sanitize.css](https://github.com/csstools/sanitize.css).
- Smart grid system based on [Jeet](https://github.com/mojotech/jeet), a human-centered precision grid.
- Declaratives breakpoints system for handling media queries.
- Flexible typography setup with strong separation of concerns.
- [Coming soon] Basic wires theme for quick UI prototyping (can be disabled).


## Installation

First, add Pickle.scss as dependency into your project:

```bash
yarn add kuizto/pickle.scss#master
```

Then, import and initiate Pickle.scss using Sass:

```scss
/* Import Pickle.scss */
@import '~pickle.scss/pickle';

/* Initiate with default config */
@include pickle;
```

🎉 Done! Pickle.scss is ready to use.


## Usage

### 🔲 Grid system

**Pickle** act as flavoured wrapper around the original [Jeet](https://github.com/mojotech/jeet) API, a smart human-centered precision grid. By default, it uses the following grid setup: `gutter: 3`, `layout-direction: LTR`, and `max-width: 1440px`. You can easily change those settings by updating the default `$grid` config:

```scss
/* Initiate with custom grid settings (optional) */
@include pickle((
	$grid: (
        gutter: 3,
        layout-direction: LTR,
        max-width: 1440px
    )
))    
```

Pickle gives you access to the below mixins. For more details on supported parameters, please refer to [Jeet API docs](https://github.com/mojotech/jeet/blob/master/docs/api.md).

```scss
/* Column with gutter | ($ratios: 1, $offset: 0, $cycle: 0) */
@include pickle-column(2/4);

/* Column without gutter | ($ratio: 1, $offset: 0, $cycle: 0) */
@include pickle-span(2/4, $cycle: 4);

/* Works similar to an offset | ($ratios: 0, $col-or-span: column) */
@include pickle-move(-1/3);

/* Disable source ordering setup previously */
@include pickle-unmove;

/* Center containers | ($max-width: map-get($pickle-grid, 'max-width'), $pad: 0) */
@include pickle-center(420px);

/* Reset styles associater with pickle-center */
@include pickle-uncenter;

/* Stack elements on top of each other | ($pad: 0, $align: false) */
@include pickle-stack;

/* Cancel pickle-stack */
@include pickle-unstack;

/* A simple/kinda-modern clearfix */
@include pickle-clearfix;
```

### 💠 Breakpoints

By default, Pickle.scss comes with the following declarative breakpoints: `phone-only`, `tablet-portrait-up`, `tablet-up`, `desktop-up`, and `big-desktop-up`.

```scss
@include pickle-respond('tablet-up') {
	/* ... */
}
```

> According the Sass guidelines, media queries should not be tied to specific devices, and privilege names such as `medium` `large` `huge` rather than `tablet` `computer` `tv`.
> 
> Pickle takes a different stand on the question, as we believe ambiguity breeds confusion. A term like `medium` is too broad and can be interpreted in many different ways, while `tablet` is more clear and easy to remember. The idea is not about being 100% accurate across all devices, but to be as declarative as possible and easy to use.

Not happy with the default naming convention? Use your own by updating the default `$breakpoints` config:

```scss
/* Initiate with custom breakpoints config (optional) */
@include pickle((
	$breakpoints: (
		'phone-only': (max-width: 599px),
		'tablet-portrait-up': (min-width: 600px),
		'tablet-up': (min-width: 900px),
		'desktop-up': (min-width: 1200px),
		'big-desktop-up': (min-width: 1800px)
	)
));
```

### 🎨 Colors

Because colors are an important element of styling, **Pickle** comes with a way to easily managed colors from a single place, and three different methods to use those within projects:

```scss
/* Initiate with colors (optional) */
@include pickle((
	$colors: ('beige': #f5f5dc)
));

/* Use project colors */
@include pickle-color('beige');
@include pickle-bgcolor('beige');
map-get($pickle-colors, 'beige');
```

### 🔤 Typography

**Pickle** proposed approach for typography styles relies on a strong separation of concerns. In practice, this mean that fonts setup, size groups, and custom styling properties, are all managed independently.

#### Fonts

```scss
/* Initiate with fonts (optional) */
@include pickle((
	$fonts: (
		'Primary': (
			files: './fonts/Helvetica.ttf' './fonts/Helvetica.woff2',
			family: #{'Helvetica', Arial, sans-serif}
		),
		'Secondary': (
			files: './Oswald-Regular.ttf',
			family: #{'Oswald', Arial, sans-serif}
		)
    )
));
```

#### Size groups

```scss
/* Initiate with sizes (optional) */
@include pickle((
	$sizes: (
		'XL': (
			base: 1.6rem,
			breakpoints: ('tablet-up': 5rem)
		)
		'M': (base: 1.2rem)
    )
));
```

#### Combined usage with custom styling

```scss
.title {
    @include pickle-typo($font: 'Primary', $size: 'XL');

    /* custom styling */
    text-decoration: underline;
}

.subtitle {
    @include pickle-typo($font: 'Secondary', $size: 'M');
}
```

#### Bonus: `Ratio` parameters

To understand the **ratio** parameters, you need to understand the scenarios in which you may want to use it:

- In case a font need to be changed to another one AFTER the build started. Fonts being designed differently, it can have an impact on your current font-size properties that will need to be updated across your entire project.
- In case you want to make the typography size groups consistant, no matter which font is used (different fonts usually means different design ratios, making a 16px size look bigger using one font family from another one).

To accomodate for those scenarios and ease developement, **Pickle** integrates a `ratio` property (equal to 1 by default) that can be attached to any font:

```scss
@include pickle((
	$sizes: (
		'M': (base: 1.2rem)
	)
	$fonts: (
		'Primary': (
			files: './Oswald-Regular.ttf',
			family: #{'Oswald', Arial, sans-serif},
			ratio: 1
		),
		'Secondary': (
			files: './Oswald-Regular.ttf',
			family: #{'Oswald', Arial, sans-serif},
			ratio: 1.5
		)
    )
));
```

Assuming the above config with `Primary` and `Secondary` fonts using the exact same `family` and `files`:

```scss
@include pickle-typo($font: 'Primary', $size: 'M');
/* |-> Output font-size = 1.2rem */

@include pickle-typo($font: 'Secondary', $size: 'M');
/* |-> Output font-size = 1.8rem */
```

### 🔌 Global variables

Sometimes it is important to have a central place to manage global styling variables, so **Pickle** config comes with a `$vars` parameter, allowing to declare and use variables globally.

```scss
/* Initiate with vars (optional) */
@include pickle((
	$vars: (
		'header-height': 5.5rem
    )
));

/* Use project vars */
.classname {
	height: map-get($pickle-vars, 'header-height');
}
```

## Default config

The below code shows the default config. All parameters are optional.

```scss
$config: (
    $grid: (
        gutter: 3,
        layout-direction: LTR,
        max-width: 1440px
    ),
    $breakpoints: (
        'phone-only': (max-width: 599px),
        'tablet-portrait-up': (min-width: 600px),
        'tablet-up': (min-width: 900px),
        'desktop-up': (min-width: 1200px),
        'big-desktop-up': (min-width: 1800px)
    ),
    $colors: (),
    $fonts: (),
    $sizes: (),
    $vars: ()
);

@include pickle($config);
```


## Roadmap

- [ ] Create basic wires theme for quick UI prototyping


## Contribute

```bash
# install parcel globally
npm install -g parcel-bundler

# install dependencies
yarn install

# run the playground
yarn playground
```

**Note:** Parcel will only watch changes applied to `playground/playground.scss`.
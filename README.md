# Pickle.scss

> 🥒 Vinegary Sass toolkit.


## Features

- [x] Typography setup that enforce separation of concerns.
- [x] Declaratives breakpoints system for handling media queries.
- [x] Opiniated default styles using [Sanitize.css](https://github.com/csstools/sanitize.css), along with custom flavors.
- [x] Handy management and usage of global colors and variables.


## Installation

### Basic

First, add Pickle as dependency into your project:

```bash
yarn add pickle.scss
```

Then, import and initiate using Sass:

```scss
/* Import Pickle */
@import 'node_modules/pickle.scss/pickle';

/* Include Pickle mixins and vars */
@include pickle;

/* Inject Pickle styles */
@include pickle-styles;
```

🎉 Done! Pickle is now ready to use.

### Vue.js

First, add Pickle as dependency into your project:

```bash
yarn add pickle.scss
```

Create `src/styles/globals.scss` with the following:

```scss
/* Import Pickle */
@import 'node_modules/pickle.scss/pickle';

/* Include Pickle mixins and vars */
@include pickle;
```

Inject the previously created file into your project via `vue.config.js`:

```javascript
module.exports = {
    css: {
        loaderOptions: {
            sass: {
                data: `@import "@/styles/globals.scss";`
            }
        }
    }
}
```

Finally, insert the below into your `App.vue`:

```html
<style lang="scss">
    /* Inject Pickle styles (only once in your project) */
    @include pickle-styles;
</style>
```

🎉 Done! Pickle is now ready to use.

### Advanced

```scss
/* Include Pickle mixins and vars (custom params) */
@include pickle(
    $breakpoints: (),
    $colors: (),
    $fonts: (),
    $sizes: (),
    $vars: ()
);

/* Inject Pickle styles (fonts only) */
@include pickle-styles(
    $sanitize: false,
    $flavors: false,
    $fonts: true
);
```


## Usage

**Table of contents**

- 💠 [Breakpoints](#-breakpoints)
- 🎨 [Colors](#-colors)
- 🔤 [Typography](#-typography)
- 🔌 [Global variables](#-global-variables)


### 💠 Breakpoints

By default, Pickle comes with the following declarative breakpoints: `phone-only`, `tablet-portrait-up`, `tablet-up`, `desktop-up`, and `big-desktop-up`.

```scss
@include pickle-respond('tablet-up') {
    /* ... */
};
```

> According the Sass guidelines, media queries should not be tied to specific devices, and privilege names such as `medium` `large` `huge` rather than `tablet` `computer` `tv`.
>
> Pickle takes a different stand on the question, as we believe ambiguity breeds confusion. A term like `medium` is too broad and can be interpreted in many different ways, while `tablet` is more clear and easy to remember. The idea is not about being 100% accurate across all devices, but to be as declarative as possible and easy to use.

Not happy with the default? You can update the default `$breakpoints` config with your own config, or by copying one of our recommended alternatives below:

```scss
/* Default naming groups */
@include pickle(
    $breakpoints: (
        'phone-only': (max-width: 599px),
        'tablet-portrait-up': (min-width: 600px),
        'tablet-up': (min-width: 900px),
        'desktop-up': (min-width: 1200px),
        'big-desktop-up': (min-width: 1800px)
    )
);

/* Sizes based naming groups */
@include pickle(
    $breakpoints: (
        'xsmall': (max-width: 575.99px),
        'small': (min-width: 576px),
        'medium': (min-width: 768px),
        'large': (min-width: 992px),
        'xlarge': (min-width: 1200px),
        'xxlarge': (min-width: 1800px)
    )
);

/* Devices category based naming groups */
@include pickle(
    $breakpoints: (
        'mobile-portrait': (max-width: 567.99px),
        'mobile-landscape': (min-width: 568px),
        'tablet-portrait': (min-width: 768px),
        'tablet-landscape': (min-width: 1024px),
        'laptop-displays': (min-width: 1366px),
        'desktop-displays': (min-width: 1680px)
    )
);
```

### 🎨 Colors

Because colors are an important element of styling, Pickle comes with a way to easily managed colors from a single place, and three different methods to use those within projects:

```scss
/* Initiate with colors */
@include pickle(
    $colors: (
        'blue': #0000ff
    )
);

/* Inherit from previously declared colors */
@include pickle(
    $colors: (
        'darkBlue': darken(map-get($pickle-colors, 'blue'), 20%)
    )
);

/* Use project colors */
@include pickle-color('blue');
@include pickle-bgcolor('blue');
map-get($pickle-colors, 'blue');
```

### 🔤 Typography

Pickle proposed approach for typography styles relies on a strong separation of concerns. In practice, this mean that fonts setup, size groups, and custom styling properties, are all managed independently.

#### Fonts

```scss
/* Initiate with fonts */
@include pickle(
    $fonts: (
        // primary font
        'Primary': (
            files: './fonts/Helvetica.ttf' './fonts/Helvetica.woff2',
            family: 'Helvetica',
            fallback: #{Arial, sans-serif}
        ),
        // primary font variant using different family name
        'PrimaryBold': (
            files: './fonts/Helvetica-Bold.ttf' './fonts/Helvetica-Bold.woff2',
            family: 'HelveticaBold',
            fallback: #{Arial, sans-serif}
        )
    )
);
```

#### Size groups

```scss
/* Initiate with sizes */
@include pickle(
    $sizes: (
        'XL': (
            base: 1.6rem,
            breakpoints: (
                'tablet-up': 5rem
            )
        ),
        'M': (base: 1.2rem)
    )
);
```

#### Combined usage with custom styling

```scss
/* Using both $font and $size */
.title {
    @include pickle-typo($font: 'Primary', $size: 'XL');
    text-decoration: underline;
}
.subtitle {
    @include pickle-typo($font: 'Secondary', $size: 'M');
}

/* Using $font and $size independently */
.xlarge {
    @include pickle-typo($size: 'XL');
}
.secondary {
    @include pickle-typo($font: 'Secondary');
}
```

#### Bonus: `Ratio` parameters

To understand the **ratio** parameters, you need to understand the scenarios in which you may want to use it:

- In case a font need to be changed to another one AFTER the build started. Fonts being designed differently, it can have an impact on your current font-size properties that will need to be updated across your entire project.
- In case you want to make the typography size groups consistant, no matter which font is used (different fonts usually means different design ratios, making a 16px size look bigger using one font family from another one).

To accomodate for those scenarios and ease developement, Pickle integrates a `ratio` property (equal to 1 by default) that can be changed for any font:

```scss
@include pickle(
    $sizes: (
        'M': (base: 1.2rem)
    ),
    $fonts: (
        'Primary': (
            files: './Oswald-Regular.ttf',
            family: 'Oswald',
            ratio: 1
        ),
        'Secondary': (
            files: './Oswald-Regular.ttf',
            family: 'Oswald',
            ratio: 1.5
        )
    )
);
```

Assuming the above config with `Primary` and `Secondary` fonts using the exact same `family` and `files`:

```scss
@include pickle-typo($font: 'Primary', $size: 'M');
/* |-> Output font-size = 1.2rem */

@include pickle-typo($font: 'Secondary', $size: 'M');
/* |-> Output font-size = 1.8rem */
```

### 🔌 Global variables

Sometimes it is important to have a central place to manage global styling variables, so Pickle config comes with a `$vars` parameter, allowing to declare and use variables globally.

```scss
/* Initiate with vars */
@include pickle(
    $vars: (
        'header-height': 5.5rem
    )
);

/* Inherit from previously declared vars */
@include pickle(
    $vars: (
        'headerLarge-height': calc(#{map-get($pickle-vars, 'header-height')} * 2)
    )
);

/* Use project vars */
map-get($pickle-vars, 'header-height');
```


## Contribute

```bash
# install Vue instant prototyping
# https://cli.vuejs.org/guide/prototyping.html
npm install -g @vue/cli-service-global
# or
yarn global add @vue/cli-service-global

# install project dependencies
yarn install

# run the playground
yarn playground
```

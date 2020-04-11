# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).


## [1.0.0] - 2020-04-12

### Breaking

- The grid system has been removed completely.
- The `$webpack` option is not necessary anymore.
- `@include pickle-once` has been replaced with `@include pickle-styles`.

### Added/Changed/Removed/Fixed

- All options can now be extended, meaning you can call `@include pickle()` multiple times with different options in the same document.
- Colors and Variables can now be referenced within the same declaration. Meaning you can declare a color that is a direct variation of another color, instantly.
- Ability to select which styles to inject from `sanitize` (consistent defaults cross-browser), `flavors` (pickle custom default styles), and `fonts` (font-face declarations). By default, all three are injected using `@include pickle-styles`.
- Fonts have two new parameters: `fallback` and `weight`. We’ve also improved the use of fonts variants.
- Typography mixin `@include pickle-typo` can now be used only with `$font` (font-family only) or `$size` (responsive sizes only).
- Pickle is now dependency-free and its overall footprint size has been reduced.
- Documentation has been updated with new examples and recommendations.
- Various bug fixes and code improvements.
- Changelog added.

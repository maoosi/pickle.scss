<template>
    <div class="container">
        <div class="column">First column</div>
        <div class="column">Second column</div>
        <div class="column">Third column</div>
    </div>
</template>

<style lang="scss">
@import '../pickle';

@include pickle(
    $colors: (
        'red': red,
        'yellow': yellow,
        'green': green
    ),
    $vars: (
        'column-height': 10rem
    )
);

@include pickle(
    $colors: (
        'darkYellow': darken(map-get($pickle-colors, 'yellow'), 20%)
    ),
    $vars: (
        'column-height-double': calc(#{map-get($pickle-vars, 'column-height')} * 2)
    )
);

@include pickle(
    $sizes: (
        'XL': (
            base: 1.6rem,
            breakpoints: (
                'tablet-up': 5rem
            )
        ),
        'S': (
            base: 1rem,
            breakpoints: (
                'tablet-up': 2rem
            )
        )
    ),
    $fonts: (
        'Primary': (
            files: './fonts/Montserrat/Montserrat-Light.ttf',
            family: 'MontserratLight',
            fallback: #{Arial, sans-serif},
            ratio: 1
        ),
        'PrimaryBold': (
            files: './fonts/Montserrat/Montserrat-Bold.ttf',
            family: 'MontserratBold',
            fallback: #{Arial, sans-serif},
            ratio: 1
        )
    )
);

@include pickle_styles(
    $sanitize: true,
    $flavors: true,
    $fonts: true
);

body {
    @include pickle-respond('tablet-up') {
        @include pickle-bgcolor('red');
    }

    .container {
        @include pickle-typo($font: 'Primary', $size: 'XL');

        .column {
            @include pickle-bgcolor('yellow');
            height: map-get($pickle-vars, 'column-height');

            &:nth-of-type(2) {
                @include pickle-bgcolor('darkYellow');
                height: map-get($pickle-vars, 'column-height-double');
                @include pickle-typo($font: 'PrimaryBold');
                @include pickle-typo($size: 'S');
            }
        }
    }
}
</style>
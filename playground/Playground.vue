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
    $grid: (
        gutter: 3,
        layout-direction: LTR,
        max-width: 1440px
    ),
    $colors: (
        'red': red,
        'yellow': yellow,
        'green': green
    ),
    $sizes: (
        'XL': (
            base: 1.6rem,
            breakpoints: ('tablet-up': 5rem)
        )
    ),
    $fonts: (
        'Primary': (
            files: './fonts/Oswald-Regular.ttf',
            family: #{'Oswald', Arial, sans-serif},
            ratio: 1
        )
    )
);

body {
    @include pickle-respond('tablet-up') {
        @include pickle-bgcolor('red');
    }

    .container {
        @include pickle-center;
        @include pickle-bgcolor('yellow');
        @include pickle-typo($font: 'Primary', $size: 'XL');

        .column {
            @include pickle-column(1/3);
            @include pickle-bgcolor('green');
        }
    }
}
</style>
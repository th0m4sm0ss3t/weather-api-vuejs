<template>
  <header class="header">
    <span class="header__symbols">☀</span>
    <span class="header__symbols">☁</span>
    <h1 class="header__title">Météo du jour</h1>
    <span class="header__symbols">🌧</span>
    <span class="header__symbols">❄</span>
  </header>
</template>

<script>
export default {
    name: 'Header',
}
</script>

<style lang="scss">
@import '../scss/vars.scss';

.header {
    font-family: $titleFont;
    font-weight: $titleWeight;
    font-size: $titleSize;
    padding: $gutter * 3 0;
    color: $navyBlue;
    text-transform: capitalize;
    display: flex;
    justify-content: center;
    align-items: center;

    &__title {
        margin: 0 $gutter;
    }

    &__symbols {
        margin: 0 $gutter;
        align-self: center;
        font-size: 3rem;
    }
}
</style>
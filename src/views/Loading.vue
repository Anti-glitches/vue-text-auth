<template>
    <div class="planetcircle">
        <div class="planetcircle__a"></div>
        <div class="planetcircle__b"></div>
    </div>
</template>
<script>
export default {
    
}
</script>
<style lang="scss">
$secondary: #67bbff;
$third: #2a86ff;

.planetcircle {
    animation: planetcircle 2s infinite linear;
    top: 0;
    z-index: 101;
    height: 100%;
    width: 100%;
    position: fixed;
    display: flex;
    justify-content: center;
    align-items: center;

    &__a, &__b {
        top: 50%;
        left: 50%;
        border-radius: 50%;
        position: absolute;
    }
    
    &__a {
        width: 3rem;
        height: 3rem;
        z-index: 1;
        margin: -1.5rem 0 0 -1.5rem;
        border: 5px solid $secondary;
        transform: rotate(45deg);
        animation: planetcircle_a 2s infinite cubic-bezier(0.76, 0, 0.24, 1);
    }
    &__b {
        width: 1.5rem;
        height: 1.5rem;
        margin: -.75rem 0 0 -.75rem;
        border: 5px solid $third;
        animation: planetcircle_b 2s .3s infinite cubic-bezier(0.76, 0, 0.24, 1);
        transform: rotate(-45deg);
    }
}
@keyframes planetcircle {
    0% {
        transform: rotate(0deg)
    } 100% {
        transform: rotate(360deg)
    }
}
@keyframes planetcircle_a {
    0% {
        transform: rotate(45deg) scaleY(1);
    } 50% {
        transform: rotate(45deg) scaleY(-1);
    }
}
@keyframes planetcircle_b {
    0% {
        transform: rotate(-45deg) scaleY(1);
    } 50% {
        transform: rotate(-45deg) scaleY(-1);
    }
}
</style>
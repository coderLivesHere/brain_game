<template>
    <div>
        <h1 id="Title" class="text-2xl text-center mb-4 text-gradient-to-r from-purple-500 to-pink-500">
            Лучшая игра для мозга
        </h1>
        <Game />
    </div>
</template>

<script>
import Game from './components/GameComponent.vue'

export default {
    name: 'App',
    components: {
        Game
    }
}
</script>

<style>
@keyframes animate {
    0% {
        transform: translateY(-45px);
        animation-timing-function: ease-in;
        opacity: 1;
    }
    24% {
        opacity: 1;
    }
    40% {
        transform: translateY(-24px);
        animation-timing-function: ease-in;
    }
    65% {
        transform: translateY(-12px);
        animation-timing-function: ease-in;
    }
    82% {
        transform: translateY(-6px);
        animation-timing-function: ease-in;
    }
    93% {
        transform: translateY(-4px);
        animation-timing-function: ease-in;
    }
    25%,
    55%,
    75%,
    87% {
        transform: translateY(0px);
        animation-timing-function: ease-out;
    }
    100% {
        transform: translateY(-45px);
        animation-timing-function: ease-out;
        opacity: 1;
    }
}

h1 
{
    text-transform: uppercase;
    display: block;
    animation: animate 2s infinite both;
}
</style>

<template>
    <section class="score">
        <p>Player <span>{{ this.winCount }}</span> x <span>{{ this.loseCount }}</span> Computer</p>
    </section>
</template>

<script>
    export default {
        props: [ "winCount", "loseCount" ]
    }
</script>

<style lang="scss">
section.score {
    border-bottom: 1px solid black;
    padding: 12px;
    font-size: 18px;

    span {
        padding: 8px;
        font-weight: bold;
        border: 1px solid black;
    }
}
</style>
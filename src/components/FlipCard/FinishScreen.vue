<template>
    <div id="finish" class="m-0 h-screen bg-slate-950">
        <h1 class="text-emerald-600 text-xl">✨ Congratulations ✨</h1>
        <h3 class="text-xs text-white my-2">{{ `Time play ${timeMinutesPlay} Minutes ${timeSecondsPlay} Seconds` }}</h3>
        <button
            class="bg-transparent hover:bg-emerald-500 text-emerald-700 font-semibold hover:text-white py-2 px-4 border border-emerald-500 hover:border-transparent rounded"
            @click="onPlayAgain">Play Again</button>
    </div>
</template>

<script>
export default {
    name: 'FinishScreen',
    components: {
    },
    props: {
        timeMinutesPlay: String,
        timeSecondsPlay: String,
    },
    emits: ["onPlayAgain"],
    methods: {
        onPlayAgain() {
            this.$emit("onPlayAgain");
        },
    },
}
</script>

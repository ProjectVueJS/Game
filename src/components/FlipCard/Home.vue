<template>
    <div id="FlipGame">
        <div id="homeFlipCart" v-if="gamePlay == 'choose'">
            <h3 class="text-red-700">Home Screen Poke Memories</h3>
            <p>Select mode to start game</p>
            <div class="modes flex justify-center">
                <button class="select-mode" @click="startWith(16)">
                    <span>4x4</span>
                    <span>Easy</span>
                </button>
                <button class="select-mode" @click="startWith(36)">
                    <span>6x6</span>
                    <span>Normal</span>
                </button>
                <button class="select-mode" @click="startWith(64)">
                    <span>8x8</span>
                    <span>Hard</span>
                </button>
                <button class="select-mode" @click="startWith(100)">
                    <span>10x10</span>
                    <span>Super Hard</span>
                </button>
            </div>
        </div>
        <PlayScreen :gamePlay="gamePlay" :totalOfCard="totalOfCard" :cardsRandom="settings.cardsRandom" @onFinish="finish"
            v-if="gamePlay == 'start'" />
        <!-- <ResultScreen v-if="gamePlay == 'finish'" /> -->
    </div>
</template>

<script>
import PlayScreen from "./PlayScreen.vue"
import { ramdomArr } from "./utils/traits";
export default ({
    name: 'HomeFlipCard',

    components: {
        PlayScreen,
    },

    data() {
        return {
            gamePlay: 'choose',
            // gamePlay: 'start',
            settings: {
                totalOfCard: 0,
                cardsRandom: [],
                timeStarted: null,
            },
            timePlay: 0,
        }
    },
    methods: {
        startWith(totalOfCard) {
            this.settings.totalOfCard = totalOfCard;
            // const fistListCards = Array.from(
            //     { length: this.settings.totalOfCard / 2 },
            //     (_, k) => { k = 1; k + 1; }
            // );

            var fistListCards = [];
            for (let i = 1; i <= (this.settings.totalOfCard / 2); i++) {
                fistListCards.push(i);
            }
            const secondCards = [...fistListCards];
            const cards = [...fistListCards, ...secondCards];
            this.settings.cardsRandom = ramdomArr(ramdomArr(cards));//x2
            this.settings.timeStarted = new Date().getTime();
            this.gamePlay = 'start';
        },
    },

    finish() {
        console.log('finish');
    }
})
</script>
<style scoped>
#FlipGame {
    text-align: center;
    -webkit-user-select: none;
    /* Safari */
    -ms-user-select: none;
    /* IE 10 and IE 11 */
    user-select: none;
    /* Standard syntax */
}

.modes .select-mode {
    width: 100px;
    margin: 4px;
    border-radius: 5px;
    display: flex;
    padding: 4px 0;
    flex-direction: column;
    background-color: brown;
}

.modes .select-mode span {
    margin: 0 auto;
}
</style>./utils/trail./utils/traits
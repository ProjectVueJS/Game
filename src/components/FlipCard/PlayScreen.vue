<template>
    <h3 class="text-red-700">PlayScreen</h3>
    <div class="PlayScreen mx-auto grid gap-2" :style="{
        width: `${(((((920 - 16 * 4) / Math.sqrt(cardsRandom.length) - 16) * 3) / 4) + 16) * Math.sqrt(cardsRandom.length)}px`,
        gridTemplateColumns: `repeat(${Math.sqrt(cardsRandom.length)}, minmax(0, 1fr))`
    }">
        <IsLoading v-if="gamePlay !== 'start'" />
        <!-- <h4>{{ cardsRandom }}</h4> -->
        <CardComponent class="" :front="'Front Card'" v-for="(v, k) in cardsRandom" :key="k" :ref="`card-${k}`"
            :CardContent="cardsRandom" :img="`pokememorie/${v}.png`" :same="same" :cardIndex="k"
            @onFlip="checkCard($event)" />
    </div>
</template>

<script>
import IsLoading from "../Models/IsLoading.vue"
import CardComponent from "./CardComponent.vue"

export default ({
    name: 'PlayScreen',
    props: {
        gamePlay: String,
        totalOfCard: Number,
        cardsRandom: {
            type: Array,
            default: function () {
                return [];
            }
        }
    },
    methods: {
        checkCard(index) {
            console.log(this.cardsRandom[index]);
            // break;
            if (this.same.length == 2) return 0;
            this.same.push(index);
            // console.log(this.$refs[`card-${index}`]);
            // console.log('refs');
            // console.log(this.same[0].index);
            // console.log(this.$refs);
            // console.log('--------');
            // if (this.same.length == 2 && this.same[0] == this.same[1]) {
            if (this.same.length == 2 && this.cardsRandom[this.same[0]] == this.cardsRandom[this.same[1]]) {
                // console.log(`Same[0] = ${this.same[0]} Same[1] = ${this.same[1]}`);
                this.$refs[`card-${this.same[0]}`][0].disableFlip();
                this.$refs[`card-${this.same[1]}`][0].disableFlip();
                this.same = [];
            }
            const disabledElements = document.querySelectorAll(
                ".card-inner.disabled"
            );
            if (disabledElements && disabledElements.length == this.cardsRandom.length) {
                // console.log('finish');
                setTimeout(() => {
                    this.$emit("onFinish");
                }, 1);
            } else if (this.same.length == 2 && this.same[0] != this.same[1]) {
                setTimeout(() => {
                    // console.log("wrong!");
                    this.$refs[`card-${this.same[0]}`][0].flipDown();
                    this.$refs[`card-${this.same[1]}`][0].flipDown();
                    this.same = [];
                }, 500);
            } else {
                return 0;
            }
        },

    },
    created() {
        // console.log(this.cardsRandom);
    },
    data() {
        return {
            same: [],
        }
    },
    components: {
        CardComponent,
        IsLoading
    },

})
</script>

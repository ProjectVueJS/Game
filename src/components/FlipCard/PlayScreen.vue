<template>
    <h3 class="text-red-700">PlayScreen</h3>
    <div class="PlayScreen mx-auto grid gap-2" :style="{
        width: `${(((((920 - 16 * 4) / Math.sqrt(cardsRandom.length) - 16) * 3) / 4) + 16) * Math.sqrt(cardsRandom.length)}px`,
        gridTemplateColumns: `repeat(${Math.sqrt(cardsRandom.length)}, minmax(0, 1fr))`
    }">
        <IsLoading v-if="gamePlay !== 'start'" />
        <!-- <h4>{{ cardsRandom }}</h4> -->
        <CardComponent class="" :front="'Front Card'" v-for="(v, k) in cardsRandom" :key="k" :ref="`card-${k}`"
            :CardContent="cardsRandom" :img="`pokememorie/${v}.png`" :same="same" :cardValue="v"  @onFlip="checkCard(k)" />
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
        checkCard(card) {
            if (this.same.length == 2) return 0;
            // this.same.push(this.cardsRandom[0]);
            console.log(this.$refs[`card-${card}`]);
            if (this.same.length == 2 && this.same[0] == this.same[1]) {
                this.$refs[`card-${card}`][0].disableFlip();
                this.$refs[`card-${card}`][0].disableFlip();
                this.same = [];
            }
            const disabledElements = document.querySelectorAll(
                ".card-inner.disabled"
            );
            if (disabledElements && disabledElements.length == this.cardsRandom.length - 2) {
                setTimeout(() => {
                    this.$emit("onFinish");
                }, 920);
            } else if (this.same.length == 2 && this.same[0] != this.same[1]) {
                console.log("wrong!");
                this.$refs[`card-${card}`][0].flipDown();
                this.$refs[`card-${card}`][0].flipDown();
                this.same = [];
                setTimeout(() => {

                });
            } else {
                return 0;
            }
        },

    },
    created() {
        console.log(this.cardsRandom);
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

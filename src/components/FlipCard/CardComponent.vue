<template>
    <div class="card inline-block" @click="flipUp" :style="{
        height: `${(920 - 16 * 4) / Math.sqrt(CardContent.length) - 16}px`,
        // width: `${(((920 - 16 * 4) / Math.sqrt(CardContent.length) - 16) * 3) / 4}px`,
        perspective: `${((((920 - 16 * 4) / Math.sqrt(CardContent.length) - 16) * 3) / 4) * 2}`
    }">
        <div class="card-inner" :class="{ disabled: isDisabled, flipped: isFlipped }">
            <div class="flip-card front flex justify-center items-center">
                <img src="pokememorie/icon_back.png" alt="">
            </div>
            <div class="flip-card back">
                <img :src="img" alt="">
            </div>
        </div>
    </div>
</template>
<!-- Script -->



<script>

export default {
    name: 'CardComponent',
    props: {
        CardContent: [Array, Object],
        front: String,
        same: Array,
        img:String,
        cardValue: Number
    },
    created() {
        
    },
    data() {
        return {
            isFlipped: false,
            isDisabled: false,
        }
    },
    methods: {
        flipUp() {
            if (this.same.length >= 2) return;
            if (this.isDisabled) return;
            this.isFlipped = !this.isFlipped;
            if (this.isFlipped) this.$emit("onFlip", this.cardValue);
        },
        flipDown() {
            // this.$el.classList.toggle('flipped');
            this.isFlipped = !this.isFlipped;
        },
        disableFlip() {
            this.isDisabled = true;
        }
    }
}
</script>

<!-- Style -->
<style scoped>
.card {
    border-radius: 5px;
    overflow: hidden;
}

.card .flipped {
    transform: rotateY(180deg);
}

.card-inner {
    cursor: pointer;
    position: relative;
    width: 100%;
    height: 100%;
    text-align: center;
    transition: transform 0.6s;
    transform-style: preserve-3d;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
}

.card .disabled {
    cursor: default;
}

.flip-card.front,
.flip-card.back {
    position: absolute;
    width: 100%;
    padding: 2px;
    height: 100%;
    -webkit-backface-visibility: hidden;
    backface-visibility: hidden;
}

.front {
    background-color: cadetblue
}

.back {
    background-color: green;
    transform: rotateY(180deg);
}

.flip-card.front img {
    width: 50%;
}
.flip-card.back img {
    width: 100%;
    object-fit: contain;
    filter: drop-shadow(0 0 15px yellow);
}
</style>

<!-- Template -->
<template>
    <div class="card inline-block" @click="flipUp" :style="{
        height: `${(920 - 16 * 4) / Math.sqrt(CardContent.length) - 16}px`,
        perspective: `${((((920 - 16 * 4) / Math.sqrt(CardContent.length) - 16) * 3) / 4) * 2}`
    }">
        <div class="card-inner" :class="{ disabled: isDisabled, flipped: isFlipped }">
            <div class="flip-card front flex justify-center items-center" :class="{ shake: isShakingFront }">
                <img src="pokememorie/icon_back.png" alt="">
            </div>
            <div class="flip-card back" :class="{ shake: isShakingBack }">
                <div class="back-content" :style="{backgroundImage: 'url(/PokeMemorie/bg.png)'}">
                    <img :src="img" alt="">
                </div>
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
        img: String,
        cardIndex: Number
    },
    data() {
        return {
            isFlipped: false,
            isDisabled: false,
            isShakingFront: false,
            isShakingBack: false
        }
    },
    methods: {
        flipUp() {
            if (this.same.length >= 2) return;
            if (this.isDisabled) return;
            this.isFlipped = !this.isFlipped;
            if (this.isFlipped) this.$emit("onFlip", this.cardIndex);
        },
        flipDown() {
            if (this.isFlipped) {
                this.isShakingBack = true;
            } else {
                this.isShakingFront = true;
            }
            setTimeout(() => {
                this.isShakingFront = false;
                this.isShakingBack = false;
                this.isFlipped = !this.isFlipped;

            }, 350);
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
    background-image: linear-gradient(to bottom, red 50%, white 50%);
    border: 3px solid black;
    border-radius: 5px;

}

.back {
    border-radius: 5px;

    background-color: green;
    transform: rotateY(180deg);
}

.flip-card.front img {
    width: 50%;
    border-radius: 5px;

}

.flip-card.back img {
    width: 100%;
    object-fit: contain;
    /* filter: drop-shadow(0 0 15px yellow); */
    border-radius: 5px;
    filter: drop-shadow(40px 45px 0px rgba(0, 0, 0, 0.5));
}
/*  */
.back-content {
    position: relative;
    width: 100%;
    height: 100%;
    overflow: hidden;
    border-radius: 5px;
    background-size: cover;
    background-position: center;
    z-index: -3; /* Đảm bảo background ở phía sau */
    box-shadow: inset 0 0 20px rgba(0, 0, 0, 0.5); /* Box shadow để tạo độ nổi */
}

.back-content img {
    width: 100%;
    height: 100%;
    object-fit: contain;
    position: relative;
    z-index: 1;
}
/* Animation */
@keyframes shake {
    0% {
        transform: translateX(0);
    }

    25% {
        transform: translateX(-5px);
    }

    50% {
        transform: translateX(5px);
    }

    75% {
        transform: translateX(-5px);
    }

    100% {
        transform: translateX(0);
    }
}

.flip-card.front.shake img {
    animation: shake 0.3s ease-in-out;
}

.flip-card.back.shake img {
    animation: shake 0.3s ease-in-out;
}
</style>

<script>
import { computed } from '@vue/runtime-core'

export default {
    props: {
        matched: {
            type: Boolean,
            default: false
        },
        position: {
            type: Number,
            required: true
        },
        value: {
            type: String,
            required: true
        },
        visible: {
            type: Boolean,
            default: false
        }
    },
    setup(props, context) {
        const flippedStyles = computed(() => {
            if (props.visible) {
                return 'is-flipped'
            }
        })

        const selectCard = () => {
            context.emit('select-card', {
                position: props.position,
                faceValue: props.value
            })
        }
        return {
            flippedStyles,
            selectCard
        }
    }
}
</script>

<template>
    <div class="card" :class="flippedStyles" @click="selectCard">
        <div class="card-side front">
            <img :src="`/Images/${value}.png`" :alt="value" />
            <img v-if="matched" src="../../public/Images/checkmark.svg" class="icon-checkmark" />
        </div>
        <div class="card-side back"></div>
    </div>
</template>

<style>
.card{
  position: relative;
  transition: 0.5s transform ease-in;
  transform-style: preserve-3d;
}

.card.is-flipped {
    transform: rotateY(180deg);
}

.card-side {
    width: 100%;
    height: 100%;
    position: absolute;
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    backface-visibility: hidden;
}

.card-side.front {
    background-color: rgb(228, 183, 101);
    color: white;
    transform: rotateY(180deg);
}

.card-side.back {
    background-image: url('../../public/Images/CardBack.png');
    color: white;
}
.icon-checkmark {
    position: absolute;
    right: 5px;
    bottom: 5px;
}
</style>
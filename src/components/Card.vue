<script>
export default{
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
    setup(props,context) {
        const selectCard = () => {
            context.emit('select-card', {
                position: props.position,
                faceValue: props.value
            })
        }
        return {
            selectCard
        }
    }
}
</script>

<template>
    <div class="card" @click="selectCard">
        <div v-if="visible" class="card-side front">
            <img :src="`/Images/${value}.png`" :alt="value" />
            <img v-if="matched" src="../../public/Images/checkmark.svg" class="icon-checkmark" />
        </div>
        <div v-else class="card-side back">
        </div>
    </div>
</template>

<style>
.card{
  position: relative;
}

.card-side {
    width: 100%;
    height: 100%;
    position: absolute;
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
}

.card-side.front {
    background-color: rgb(228, 183, 101);
    color: white;
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
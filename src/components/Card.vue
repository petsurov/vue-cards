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
            type: Number,
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
            {{ value }} - {{ matched }}
        </div>
        <div v-else class="card-side back">
            Back
        </div>
    </div>
</template>

<style>
.card{
  border: 5px solid #ccc;
  position: relative;
}

.card-side {
    width: 100%;
    height: 100%;
    position: absolute;
}

.card-side.front {
    background: red;
    color: white;
}

.card-side.back {
    background: blue;
    color: white;
}
</style>
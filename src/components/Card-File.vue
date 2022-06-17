<script>
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
    visible : {
      type: Boolean,
      default: false
    }
  },
  setup(props, context) {
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
    <div v-if="visible" class="card-face is-front">
      <img :src="`../assets/images/${value}.jpg`" :alt="value" />
      <img v-if="matched" src="../assets/images/done.svg"  class="icon-checkmark" />
    </div>
        <div v-else class="card-face is-back">
    </div>
  </div>

</template>

<style>

  .card {
    position: relative;
  }

  .card-face {
    width: 100%;
    height: 100%;
    position: absolute;
    border-radius: 8px;
  }

  .card-face.is-front {
    background-image: url('../assets/images/card-bg.jpg');
    color: white;

  }

  .card-face.is-back {
    background-image: url('../assets/images/popsicle-background.jpg');
    background-repeat: no-repeat;
    background-position: center;
    background-size: 100%;
    color: white;

  }

  .icon-checkmark {
    position: absolute;
    right: 5px;
    bottom: 5px;
    width: 18%;
    height: 16%;
  }

</style>

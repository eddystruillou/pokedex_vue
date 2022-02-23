<template>
  <div class="button">
    <button @click="previousPokemon">
      <i class="arrow left"></i>
    </button>
    <button @click="nextPokemon">
      <i class="arrow right"></i>
    </button>
  </div>
</template>

<script>
export default {
  emits: ['previous-pokemon', 'next-pokemon'],
  name: "navBar.vue",
  setup(props, {emit}) {
    let previousPokemon = function () {
      emit('previous-pokemon');
    }

    let nextPokemon = function () {
      emit('next-pokemon');
    }

    return {
      previousPokemon,
      nextPokemon
    }
  }
}
</script>

<style scoped>
.button {
  position: absolute;
  width: 100%;
  display: flex;
  justify-content: space-around;
  margin-left: -10px;
  top: 380px;
}

.arrow {
  border: solid black;
  border-width: 0 3px 3px 0;
  display: inline-block;
  padding: 3px;
}

.left {
  transform: rotate(135deg);
  -webkit-transform: rotate(135deg);
}

.right {
  transform: rotate(-45deg);
  -webkit-transform: rotate(-45deg);
}

button {
  border-radius: 50px;
  width: 50px;
  height: 50px;
}
</style>

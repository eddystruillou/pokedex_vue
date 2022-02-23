<template>
  <div class="description">
    <div class="stats">
      <h3>Base Stats</h3>
      <ul>
        <li v-for="stat in stats" v-bind:key="stat.base_stat + stat.stat.name">
          <span class="titleStat">{{ stat.stat.name }} :</span>
        </li>
        <li v-for="(item,index) in stats" v-bind:key="index">
          <span class="valueStat">{{ item.base_stat }}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "Description.vue",
  props: {
    stats: {
      type: Array,
      required: true
    }
  }
}
</script>

<style scoped>
.description {
  border: 2px solid black;
  border-radius: 20px 20px 0px 0px;
  background-color: rgb(231, 231, 231);
  width: 100%;
  text-align: left;
  padding: 10px;
}

.stats {
  margin-top: 10%;
}

ul {
  list-style: none;
  padding-inline-start: 0;
}
</style>

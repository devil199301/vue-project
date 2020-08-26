<template>
  <div class="home">
    <ul id="nav">
      <li @click="show = 'line'" :class="{ active: show === 'line' }">
        1. 按年份所有數量
      </li>
      <li @click="show = 'pie'" :class="{ active: show === 'pie' }">
        2. 依據總量佔比
      </li>
      <li @click="show = 'bar'" :class="{ active: show === 'bar' }">
        3. 依 kw 由小到大排序
      </li>
    </ul>
    <LineChart v-if="show === 'line'" />
    <PieChart v-if="show === 'pie'" />
    <BarChart v-if="show === 'bar'" />
  </div>
</template>

<script>
// @ is an alias to /src
import LineChart from "@/components/LineChart.vue";
import PieChart from "@/components/PieChart.vue";
import BarChart from "@/components/BarChart.vue";

export default {
  name: "Home",
  data: function() {
    return {
      show: "pie"
    };
  },
  components: {
    LineChart,
    PieChart,
    BarChart
  }
};
</script>

<style lang="sass" scoped>
#nav
  padding: 30px
  li
    display: inline-block
    list-style: none
    color: #2c3e50
    cursor: pointer
    vertical-align: top

    &+li
      margin-left: 20px

    &.active
      color: #42b983
</style>

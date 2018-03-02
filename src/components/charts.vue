<template>
  <div>
    <input type="text" v-model="text">
    <t-table fname="text" />
    <h1>Charts</h1>
    <schart :canvasId="canvasId"
      :type="type"
      :width="width"
      :height="height"
      :data="data"
      :options="options"
    ></schart>
    <q-btn @click="update">Update</q-btn>
  </div>
</template>


<script>
import datatable from 'components/datatable.vue'
console.log(datatable)
import Schart from 'vue-schart'
export default {
  components: {
    Schart,
    't-table': datatable
  },
  data () {
    return {
      text: '',
      canvasId: 'myCanvas',
      type: 'bar',
      width: 500,
      height: 400,
      data: [
        {name: '2014', value: 1342},
        {name: '2015', value: 2123},
        {name: '2016', value: 1654},
        {name: '2017', value: 1795}
      ],
      options: {
        title: 'Total sales of stores in recent years'
      }
    }
  },
  methods: {
    update () {
      this.data = [
        {name: 'asd', value: 2313},
        {name: 'asd', value: 2142},
        {name: 'asd', value: 1214},
        {name: 'asd', value: 205}
      ]
    }
  }
}
</script>

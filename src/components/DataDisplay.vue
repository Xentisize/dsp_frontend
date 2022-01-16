<template>
  <div>
    <h2>{{ meta["verbose_name"] }}</h2>
    <small>{{ meta["description"] }}</small>
    <div>
      <span class="badge bg-primary" v-for="col in meta['columns']" :key="col" style="margin-right: 5px">
        {{ col }} 
      </span>
    </div>  

    <table class="table" v-if="Object.keys(meta).length !== 0">
      <thead>
        <tr>
          <th>Category</th>
          <th>Year</th>
          <th>Region</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>{{ meta["category"] }}</td>
          <td>{{ meta["year"] }}</td>
          <td>{{ meta["region"] }}</td>
        </tr>
      </tbody>
    </table>

    <DescriptiveTable :data="meta['descriptive_data']" v-if="Object.keys(meta).length !== 0"></DescriptiveTable>    

    <img class="img-fluid" :src="'http://localhost:8000/figures/' + meta['descriptive_plot']" v-if="meta['descriptive_plot']">

  </div>
</template>

<script>
import DescriptiveTable from "./DescriptiveTable.vue"

export default {
  name: "DataDisplay",

  components: {DescriptiveTable},

  props: {
    source: String,
    meta: Object
  },

  // data() {
  //   return {
  //     meta_data: {}
  //   }
  // },

  // methods: {
  //   getFileMetaData(data_source) {
  //     let base_url = "http://localhost:8000/data/show?file="
  //     let data_url = base_url + data_source
  //     fetch(data_url).then(response => response.json()).then(
  //       data => {
  //         this.meta_data = data
  //       }
  //     )
  //   }
  // },

  // watch: {
  //   source: function(val) {
  //     this.getFileMetaData(val)
  //   }
  // }
}
</script>

<style>

</style>
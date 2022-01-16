<template>
  <div class="row">
    <div class="row">
      <div class="col">
        <DataList :list="file_list" @fileSelectedChange="change_file_selected"></DataList>
      </div>
    </div>
    <div class="row">
      <div class="col-sm-7">
      <DataDisplay :meta="meta_data"></DataDisplay>
      </div>
      <div class="col-sm-5">
        <GraphZone :meta="meta_data"></GraphZone>
      </div>
    </div>
  </div>
</template>

<script>
import DataList from "./DataList.vue"
import DataDisplay from "./DataDisplay.vue"
import GraphZone from "./GraphZone.vue"

export default {
  name: "MainPanel",

  components: {
    DataList,
    DataDisplay,
    GraphZone
  },

  data() {
    return {
      file_list: "",
      file_selected: "",
      meta_data: {},
    }
  },

  methods: {
    change_file_selected(file) {
      this.file_selected = file
    },

    getFileMetaData(data_source) {
      let base_url = "http://localhost:8000/data/show?file="
      let data_url = base_url + data_source
      fetch(data_url).then(response => response.json()).then(
        data => {
          this.meta_data = data
        }
      )
    }
  },

  watch: {
    file_selected: function(val) {
      this.getFileMetaData(val)
    }
  },

  beforeCreate() {
    fetch("http://localhost:8000/data/list_files").then(
      response => response.json()
    ).then(data => {
      this.file_list = data
    })
  }
}
</script>

<style>

</style>
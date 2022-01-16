<template>
  <select name="file-selected" class="form-select" v-model="file_selected">
    <option v-for="l in list" :key="l">
      {{ l }}
    </option>
  </select>
 
</template>

<script>
export default {
  name: "DataList",

  props: ["list"],

  data() {
    return {
      file_selected: "",
    }
  },

  watch: {
    file_selected: function(val) {
      this.$emit("fileSelectedChange", val)
    }
  }

}
</script>

<style>

</style>
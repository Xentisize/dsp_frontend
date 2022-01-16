<template>
  <table class="table table-striped">
    <thead>
      <tr>
        <th>Feature</th>
        <th v-for="(h, i) in headers" :key="i">{{ h }}</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(d, i) in data" :key="i">
        <td><strong>{{ i }}</strong></td>
        <td>{{ d["25%"].toFixed(3) }}</td>
        <td>{{ d["50%"].toFixed(3) }}</td>
        <td>{{ d["75%"].toFixed(3) }}</td>
        <td>{{ d["count"].toFixed(3) }}</td>
        <td>{{ d["max"].toFixed(3) }}</td>
        <td>{{ d["mean"].toFixed(3) }}</td>
        <td>{{ d["min"].toFixed(3) }}</td>
        <td>{{ d["std"].toFixed(3) }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  name: "DescriptiveTable",

  props: {
    data: Object,
  },

  data() {
    return {
      row_headers: [],
      headers: ["25%", "50%", "75%", "count", "max", "mean", "min", "std"],
    }
  },

  methods: {
    getRowHeader(data) {
      this.row_headers = Object.keys(data)
    }
  },

  beforeMount() {
    this.getRowHeader(this.data)
  },

  watch: {
    data: function(val) {
      this.getRowHeader(val)
    }
  }
}
</script>

<style>

</style>
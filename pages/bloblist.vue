<template>
  <div class="list container">
    <el-table :data="list" @row-click="listclick">
      <el-table-column prop="blobId" label="ID"></el-table-column>
      <el-table-column prop="blobName" label="BLOB NAME"></el-table-column>
      <el-table-column prop="blobTypeName" label="BLOB TYPE"></el-table-column>
    </el-table>

    <div class="links">
      <el-button @click="load" plain>View More</el-button>
    </div>
  </div>
</template>
<script>
import axios from "axios";

// import ListView from '@/components/ListView';
export default {
  async asyncData({ params, $axios, app }) {
    let list;
    if (process.server) list = await $axios.get("list?page=0");
    else list = await axios.get("list?page=0");
    return {
      list: list.data.data,
      page: 0,
      asyncInit: process.server
    };
  },
  methods: {
    listclick(row, event, column) {
      location.href = "blob/" + row.blobId;
    },
    load() {
      this.page = this.page + 1;
      axios.get("list?page=" + this.page).then(res => {
        res.data.forEach(val => {
          this.list.push(val);
        });
      });
    }
  },
  components: {
    // ListView
  }
};
</script>
<style lang="scss">
.container {
  margin: 0 auto;
  min-height: 100vh;
  text-align: center;
  justify-content: center;
}
</style>


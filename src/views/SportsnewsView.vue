<template>
  <div>
    <h1>体育资讯展示</h1>
    <el-table :data="sportsnewsTableData" style="width: 100%">
      <el-table-column prop="title" label="资讯标题"></el-table-column>
      <el-table-column prop="content" label="资讯内容"></el-table-column>
      <el-table-column :formatter="formatDate" prop="publishtime" label="发布时间"></el-table-column>
    </el-table>
    <el-pagination
        background
        :current-page="sportsnewsPageNum"
        page-size="5"
        @current-change="changeSportsnewsPageNum"
        :page-count="sportsnewsPages"
        layout="prev, pager, next"
        :total="sportsnewsTotal"
    ></el-pagination>
  </div>
</template>

<script>
import { getSportsnewsPage } from '@/api/app';

export default {
  name: 'SportsnewsView',
  data() {
    return {
      sportsnewsTableData: [],
      sportsnewsPages: 0,
      sportsnewsTotal: 0,
      sportsnewsPageNum: 1,
    };
  },
  methods: {
    changeSportsnewsPageNum(value) {
      this.sportsnewsPageNum = value;
      this.getSportsnewsPage();
    },
    getSportsnewsPage() {
      getSportsnewsPage({}, this.sportsnewsPageNum).then((res) => {
        if (res.code === 2000) {
          this.sportsnewsTableData = res.data.list;
          this.sportsnewsPageNum = res.data.pageNum;
          this.sportsnewsPages = res.data.pages;
          this.sportsnewsTotal = res.data.total;
        }
      });
    },
    formatDate(row, column, cellValue) {
      if (cellValue) {
        const date = new Date(cellValue);
        const year = date.getFullYear();
        const month = String(date.getMonth() + 1).padStart(2, '0');
        const day = String(date.getDate()).padStart(2, '0');
        const hours = String(date.getHours()).padStart(2, '0');
        const minutes = String(date.getMinutes()).padStart(2, '0');
        const seconds = String(date.getSeconds()).padStart(2, '0');
        return `${year}-${month}-${day} ${hours}:${minutes}:${seconds}`;
      }
      return '';
    },
  },
  created() {
    this.getSportsnewsPage();
  },
};
</script>
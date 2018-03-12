<template>
<el-container>
  <el-container>
    <el-header style="font-size: 24px; text-align: center">
      <span>Hall of Shame - Argentina IT</span>
    </el-header>
    
    <el-main>
      <el-table :data="companies">
        <el-table-column prop="name" label="Nombre" width="140">
        </el-table-column>
      </el-table>
    </el-main>
  </el-container>
</el-container>
</template>

<script>
export default {
  methods: {
    loadCompanies() {
      fetch(
        'http://cors-anywhere.herokuapp.com/http://openqube.io/api/ranking?limit=100&sort=+average_score&reviews.total%3E=10',
      )
        .then(response => response.json())
        .then(companies => (this.companies = companies));
    },
  },
  mounted() {
    this.loadCompanies();
  },
  data() {
    return {
      companies: [],
    };
  },
};
</script>
  .el-header {
    background-color: #B3C0D1;
    color: #333;
    line-height: 60px;
  }
  
  .el-aside {
    color: #333;
  }
</style>

<template>
<el-container>
  <el-container>
	  <el-header style="font-size: 24px; text-align: center"><span>Hall of Shame - Argentina IT</span></el-header>
    <el-main>
<!--<el-menu default-active="2" class="el-menu-vertical-demo">-->
  <!--<el-menu-item index="2">-->
    <!--<i class="el-icon-menu"></i>-->
    <!--<span slot="title">Inicio</span>-->
  <!--</el-menu-item>-->
  <!--<el-menu-item index="4">-->
    <!--<i class="el-icon-setting"></i>-->
    <!--<span slot="title">Acerca de</span>-->
  <!--</el-menu-item>-->
<!--</el-menu>-->
      <el-table :data="companies" @expand-change="handleCompanyExpansion">
	 <el-table-column type="expand">
      <template slot-scope="props">
	      <div v-for="comment in props.row.comments">
		      <i class="el-icon-caret-right"></i>
		      <span>{{ comment  }}</span>
	      </div>
      </template>
    </el-table-column>
        <el-table-column prop="name" label="Nombre" width="300" sortable>
        </el-table-column>
        <el-table-column prop="scores.benefits" label="Beneficios" width="300" sortable>
        </el-table-column>
	<el-table-column prop="scores.code_quality" label="Code quality" width="300" sortable>
        </el-table-column>
	<el-table-column prop="scores.equipment" label="Equipamiento" width="300" sortable>
        </el-table-column>
	<el-table-column prop="scores.salary" label="Salario" width="300" sortable>
        </el-table-column>
	<el-table-column label="Openqube" width="300">
      <template slot-scope="scope">
   	<el-button
          size="mini">
		<a class="link" :href="getOpenQubeLink(scope.row)" target="_blank">Reseñas</a></el-button>
      </template>
</el-table-column>
</el-table>
</el-main>
</el-container>
</el-container>
</template>

<script>
export default {
  methods: {
    handleCompanyExpansion(company) {
      this.getReviews(company);
    },
    loadCompanies() {
      fetch(
        'https://cors-anywhere.herokuapp.com/http://openqube.io/api/ranking?limit=100&sort=+average_score&reviews.total%3E=10',
      )
        .then(response => response.json())
        .then(companies => (this.companies = companies));
    },
    getOpenQubeLink(company) {
      return `http://openqube.io/companies/${company._id}`;
    },
    async getReviews(company) {
      const {_id} = company;

      const response = await fetch(
        `https://cors-anywhere.herokuapp.com/http://openqube.io//api/reviews?company_id=${_id}&limit=5&status=%2Fapproved%7Cadded%2F&offset=0&sort=-added`,
      );

      const reviewsOfCompany = await response.json();

      this.companies = this.companies.map(item => {
        if (company._id !== item._id) return item;

        const comments = reviewsOfCompany.map(
          review => review.comments.negative,
        );

        company.comments = comments;
        return company;
      });
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
<style>
body,
html {
  font-family: Helvetica Neue, Helvetica, PingFang SC, Hiragino Sans GB,
    Microsoft YaHei, SimSun, sans-serif;
  font-weight: 400;
}

.el-header {
  background-color: #b3c0d1;
  color: #333;
  line-height: 60px;
}

.el-aside {
  color: #333;
}

a.link {
  color: inherit;
  text-decoration: none;
}
</style>

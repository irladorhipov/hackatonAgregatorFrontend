<template>
  <TheNavbar />

  <TheIntro />

  <main>
    <div class="container">
      <AppVacancyFilters :inputData="filtredData" @filter="filterArrSalary" />
      <AppVacancyList :inputData="filtredData" />
    </div>
  </main>
  <ui-pagination
  v-model="page"
  :total="total"
  :pageSize="pageSize"
  show-total
  position="center"
></ui-pagination>
</template>

<script>
import axios from "axios";
import TheNavbar from "./components/TheNavbar.vue";
import TheIntro from "./components/TheIntro.vue";
import AppVacancyList from "@/components/vacancy/AppVacancyList.vue";
import AppVacancyFilters from "@/components/vacancy/AppVacancyFilters.vue";

export default {
  components: { TheNavbar, TheIntro, AppVacancyList, AppVacancyFilters },
  data() {
    return {
      inputData: [],
      filtredData: [],
      page: 1,
      total: null,
      pageSize: 50,
    };
  },
  created() {
    this.fetchVacancies();
  },
  watch: {
   page: function() {
    this.fetchVacancies()
   }
  }, 
  methods: {
    filterArrSalary(val) {
      return (this.filtredData = this.inputData.filter(function (item) {
        return item.vacancy.salary_min > val;
      }));
    },
    fetchVacancies() {
      axios
        .post("https://landing.sparky-application.ru/api/vacancies-paginate", {
          page: this.page
        })
        .then((res) => {
          console.log(res)
          // this.page = res.data.current_page;
          this.total = res.data.total;
          this.inputData = res.data.data;
          this.filtredData = res.data.data;
        })
        .catch((err) => {
          console.log('Failed to get vacancies', err);
        });
    },
  },
};
</script>

<style>
:root {
  --color-primary: #2563eb;
  --color-primary-hover: #3f7af8;
  --color-primary-pastel: #c5d5f9;
  --color-primary-text: white;

  --color-border: #d1d5db;

  --transition: .3s ease-in-out;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

#app {
  font-family: 'Helvetica Neue', 'Onest', 'SF Pro', 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.container {
  padding: 0 16px;
  max-width: 1200px !important;
  margin: 0 auto;
}

main .container {
  padding: 20px 16px;
  display: flex;
  flex-direction: column;
}

main :deep(aside) {
  width: 400px;
  min-width: 400px;
}

main :deep(section) {
  width: 100%;
}

@media screen and (min-width: 968px) {
  main .container {
    flex-direction: row;
  }
}</style>

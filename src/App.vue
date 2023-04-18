<template>
  <navbarapp />
  <div class="container">
    <vacanciesSearch :data="data" />
  </div>
</template>

<script>
import axios from 'axios'

import navbarapp from '@/components/UI/NavbarApp.vue'
import vacanciesSearch from '@/components/UI/VacanciesSearch.vue'
export default {
    components: {navbarapp, vacanciesSearch},
    data() {
        return {
            data: [],
        }
    },
    created() {
        this.vacancies()
      },
      methods: {
          vacancies() {
              axios.post('https://landing.sparky-application.ru/api/vacancies-paginate')
              .then((res) => {
                this.data = res.data.data
                  console.log(res)
              })
              .catch((err) => {
                  console.log(err)
              })
          }
      }
}
</script>


<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.navbar {
  background-color: #6200ee;
  color: #fff;
  padding: 25px;
}

.input-search {
  margin-top: 30px;
  outline: none;
  border: none;
  width: 100%;
  border-radius: 4px;
  padding: 20px;
  font-size: 20px;
  -webkit-box-shadow: 4px 4px 8px 0px rgba(34, 60, 80, 0.2);
  -moz-box-shadow: 4px 4px 8px 0px rgba(34, 60, 80, 0.2);
  box-shadow: 4px 4px 8px 0px rgba(34, 60, 80, 0.2);
}

.container {
  max-width: 1200px;
  margin: 0 auto;
}

.vacancy {
  margin-top: 30px;
  border-radius: 4px;
  border: 3px solid #6200ee;
  padding: 30px;
}

.text-content {
  margin: 30px 0;
  padding: 30px;
  border-radius: 4px;
  border: 2px solid #000;
}

.demo-grid.max-width {
  max-width: 1280px;
}

</style>

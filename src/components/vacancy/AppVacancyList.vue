<template>
  <section class="section section-vacancies">
    <input
      v-model="search"
      class="section-vacancies__search"
      role="searchbox"
      aria-placeholder="Поиск по вакансиям"
      placeholder="Поиск по вакансиям"
      type="search"
      maxlength="255"
      tabindex="0"
    />

    <div v-if="vacancies.length > 0" class="section-vacancies__list">
      <AppVacancyCard
        v-for="(vacancy, index) in vacancies"
        :key="vacancy.id"
        :item="vacancy"
        :tabindex="index + 1"
      />
    </div>
    <h2 v-else>
      Ничего не найдено
    </h2>
  </section>
</template>

<script>
import AppVacancyCard from "@/components/vacancy/AppVacancyCard.vue";

export default {
  components: { AppVacancyCard },
  props: ["inputData"],
  computed: {
    vacancies() {
      let self = this;
      return this.inputData.filter(function (item) {
        return (
          item.vacancy.title.toLowerCase().includes(self.search.toLowerCase()) ||
          item.vacancy.raw_text.toLowerCase().includes(self.search.toLowerCase())
        );
      });
    },
  },
  data() {
    return {
      search: "",
    };
  },
};
</script>

<style scoped>
.section-vacancies {
  width: 100%;

  padding: 0 20px;
}
.section-vacancies__search {
  width: 100%;

  margin-bottom: 8px;

  padding: 8px 16px;

  background: white;

  outline: none;
  border: 2px solid var(--color-border);
  border-radius: 12px;

  font-size: 20px;
  font-weight: 500;

  transition: var(--transition);
  transition-property: border-color;
}
.section-vacancies__search:focus {
  border-color: var(--color-primary);
}
.section-vacancies__list {
  display: flex;
  flex-direction: column;
}
</style>
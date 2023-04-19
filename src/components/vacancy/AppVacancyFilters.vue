<template>
  <aside class="filters" role="form">
    <h2 class="filters__title">
      Фильтры
    </h2>
    <div class="filters__prop">
      <p class="filters__prop_title">
        Зарплата
      </p>
      <input
        @change="this.filterArr()"
        v-model="rangeVal"
        class="filters__prop_input"
        type="range"
        min="0"
        :max="salaryMax.vacancy?.salary_max"
      />
      <div class="flex justify-between">
        <span class="inline-block text-lg font-bold text-blue-400">
          0
        </span>
        <span class="inline-block text-lg font-bold text-blue-400">
          {{ salaryMax.vacancy?.salary_max ?? 100000 }}
        </span>
      </div>
    </div>
  </aside>
</template>

<script>
export default {
  props: ["inputData"],
  emits: ["filter"],
  data() {
    return {
      rangeVal: 0,
    };
  },
  methods: {
    filterArr() {
      this.$emit("filter", this.rangeVal);
    },
  },
  computed: {
    salaryMax() {
      let arr = this.inputData;

      if (arr.length > 0) {
        var max = arr.reduce((prev, cur) => {
          if (prev.vacancy.salary_max > cur.vacancy.salary_max) {
            return prev;
          }
          return cur;
        });

        return max;
      } else {
        return {};
      }
    },
  },
};
</script>

<style scoped>
.filters {
  width: 100%;

  padding: 20px;

  margin-bottom: 12px;

  height: max-content;

  border: 2px solid var(--color-border);
  border-radius: 12px;
}
@media screen and (min-width: 968px) {
  .filters {
    min-width: 300px;
    max-width: 300px;
  }
}
.filters__title {
  margin-bottom: 12px;

  font-size: 22px;
  font-weight: 500;
  text-align: center;
}

.filters__prop {
  margin-bottom: 16px;

  display: flex;
  flex-direction: column;
}
.filters__prop:last-child {
  margin-bottom: 0;
}
.filters__prop_title {
  margin-bottom: 12px;

  font-size: 16px;
  font-weight: 500;

  color: rgba(0, 0, 0, 0.5);
}
.filters__prop_input {
  width: 100%;
  height: auto;

  margin-bottom: 8px;
}
.filters__prop_input[type="range"] {
  height: 4px;

  background: var(--color-primary-pastel);

  border-radius: 50px;

  appearance: none;

  cursor: pointer;
}
</style>
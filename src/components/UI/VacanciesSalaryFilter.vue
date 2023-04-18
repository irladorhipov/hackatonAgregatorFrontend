<template>
  <div
    class="p-4 mb-5 bg-white border border-gray-200 dark:bg-gray-900 dark:border-gray-900"
  >
    <h2 class="text-2xl font-bold dark:text-gray-400">Зарплата</h2>
    <div class="w-16 pb-2 mb-6"></div>
    <div>
      <input
        type="range"
        v-model="rangeVal"
        class="w-full h-1 mb-4 bg-blue-100 rounded appearance-none cursor-pointer"
        min="0"
        :max="salaryMax.vacancy?.salary_max"
        @change="this.filterArr()"
      />
      <div class="flex justify-between">
        <span class="inline-block text-lg font-bold text-blue-400">0</span>
        <span class="inline-block text-lg font-bold text-blue-400">{{
          salaryMax.vacancy?.salary_max ?? 100000
        }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["inputData"],
  methods: {
    filterArr() {
      this.$emit("filterArr", this.rangeVal);
    }
  },
  computed: {
    salaryMax() {
    
      var arr = this.inputData;

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
  data() {
    return {
      rangeVal: 0,
    };
  },
};
</script>

<style>
</style>
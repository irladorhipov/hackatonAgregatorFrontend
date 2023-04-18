<template>
    <div class="vacancy">
       <b class="vacancy-title">{{ item.vacancy.title }}</b>
       <div class="text-content">
        {{ getTextHTML(item.vacancy.raw_text) }}
       </div>
       <ui-button @click="show" raised>Перейти</ui-button>
    </div>
</template>

<script>
export default {
    props: ['item'],
    data() {

    },
    methods: {
        getTextHTML(text) {
            let data = new DOMParser().parseFromString(text, "text/html").documentElement.textContent
            return data;
        },
        show() {
      this.$confirm({
        message: `Вы точно хотите перейти к вакансии ${this.item.vacancy.title} ?`,
        acceptText: 'Да',
        cancelText: 'Нет'
      }).then((result) => {
        if (result) {
            window.location.href = this.item.vacancy.link
        }
      });
    }
    }
}
</script>

<style>

</style>
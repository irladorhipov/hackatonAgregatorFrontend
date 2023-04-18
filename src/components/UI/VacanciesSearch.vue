<template>
    <div>
        <input type="text" v-model="search" placeholder="Поиск по всем полям" class="input-search">
    </div>
    <vacancy v-for="item in filterArr" :key="item" :item="item" />
    <h2 v-if="!filterArr.length">Ничего не найдено</h2>
</template>

<script>
import vacancy from '@/components/Items/VacancyApp.vue'
export default {
    components: {vacancy},
    props: ['data'],
    computed: {
        filterArr() {
            let self = this;
            return this.data.filter(function (item) {
                return item.vacancy.title.toLowerCase().includes(self.search.toLowerCase()) ||
                    item.vacancy.raw_text.toLowerCase().includes(self.search.toLowerCase())
            })
        }
    },
    data() {
        return {
            search: '',
        }
    }
}
</script>

<style>

</style>
<template>
    <div class="main">
        <img :src="getImageUrl('images/title/digimon-list.png')" class="title" alt="Digimon List">
        <div class="main-layout">
            <DigimonListFilter class="filter-section" @filter-change="updateFilters" />
            <div class="list-section">
                <DigimonList :filters="filters" @digimon-selected="handleDigimonSelected" />
            </div>
        </div>
    </div>
</template>

<script>
import DigimonListFilter from './DigimonListFilter.vue'
import DigimonList from './DigimonList.vue'

export default {
    components: {
        DigimonListFilter,
        DigimonList,
    },
    data() {
        return {
            filters: {
                stages: [],
                types: [],
                attributes: []
            }
        };
    },
    methods: {
        updateFilters(newFilters) {
            this.filters = {
                stages: newFilters.stages || [],
                types: newFilters.types || [],
                attributes: newFilters.attributes || []
            };
        },
        handleDigimonSelected(digimonNumber) {
            this.$emit('digimon-selected', digimonNumber);
        },
        getImageUrl(imagePath) {
            return `${process.env.VUE_APP_API_URL}${imagePath}`;
        }
    }
}
</script>

<style scoped>
.main-layout {
    display: flex;
}

.main {
    text-align: center;
    width: 1450px;
    margin-left: auto;
    margin-right: auto;
}

.filter-section {
    flex: 1 1 20%;
    background-color: #fff;
}

.list-section {
    flex: 1 1 80%;
    background-color: #fff;
}

.title {
    width: 500px;
    margin-bottom: 50px;
}
</style>
<template>
    <div class="main">
        <img :src="`http://localhost:9706/images/title/digimon-details.png`" class="title" alt="Digimon Details">
        <div class="main-layout">
            <DigimonInfo @back="handleBack" :digimon="digimonDetails" class="left-section" />
            <div class="right-section">
                <DigimonDescription :digimon="digimonDetails" />
                <DigimonEvolutionChart :digimon="digimonDetails" @digimon-selected="handleDigimonSelected" />
            </div>
        </div>
    </div>
</template>

<script>
import DigimonDescription from './DigimonDescription.vue'
import DigimonInfo from './DigimonInfo.vue'
import DigimonEvolutionChart from './DigimonEvolutionChart.vue';
import axios from '../../axios';

export default {
    props: ['digimonNumber'],
    components: {
        DigimonDescription,
        DigimonInfo,
        DigimonEvolutionChart
    },
    data() {
        return {
            digimonDetails: null
        }
    },
    watch: {
        digimonNumber: 'fetchDigimonDetails'
    },
    mounted() {
        this.fetchDigimonDetails();
    },
    methods: {
        async fetchDigimonDetails() {
            const requestBody = { number: this.digimonNumber }
            try {
                const response = await axios.post('/digimon/details', requestBody, {
                    headers: {
                        'Content-Type': 'application/json'
                    }
                });
                this.digimonDetails = response.data
            } catch (error) {
                console.error('Error fetching Digimon details:', error);
            }
        },
        handleBack() {
            this.$emit('back');
        },
        handleDigimonSelected(digimonNumber) {
            this.$emit('digimon-selected', digimonNumber);
        }
    }
}
</script>

<style scoped>
.main-layout {
    display: flex;
    gap: 50px;
}

.main {
    text-align: center;
    width: 1450px;
    margin-left: auto;
    margin-right: auto;
    margin-bottom: 50px;
}

.left-section {
    flex: 1 1 33%;
    background-color: #fff;
}

.right-section {
    flex: 1 1 67%;
    background-color: #fff;
}

.title {
    width: 650px;
    margin-bottom: 50px;
}
</style>
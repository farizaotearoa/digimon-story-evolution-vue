<template>
  <div id="app">
    <DigimonHeader />
    <div v-if="!selectedDigimonNumber">
      <DigimonList @digimon-selected="handleDigimonSelected"/>
    </div>
    <div v-else>
      <DigimonDetails :digimon-number="selectedDigimonNumber" @back="handleBack"/>
    </div>
    <DigimonFooter />
  </div>
</template>

<script>
import DigimonHeader from './components/DigimonHeader.vue'
import DigimonList from './components/DigimonList/DigimonListLayout.vue';
import DigimonDetails from './components/DigimonDetails/DigimonDetailsLayout.vue';
import DigimonFooter from './components/DigimonFooter.vue'

export default {
  name: 'App',
  components: {
    DigimonHeader,
    DigimonList,
    DigimonDetails,
    DigimonFooter
  },
  data() {
    return {
      selectedDigimonNumber: null
    };
  },
  methods: {
    handleDigimonSelected(digimonNumber) {
      this.selectedDigimonNumber = digimonNumber;
    },
    handleBack() {
      console.log('back');
      this.selectedDigimonNumber = null;
    }
  }
};
</script>

<style>
body {
  margin: 0px;
  padding: 10px;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  display: flex;
  flex-direction: column;
}
</style>

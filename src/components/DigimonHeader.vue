<template>
    <div class="title">
        <img :src="getImageUrl('images/title/digimon-title.png')" class="image" alt="Digimon title">
    </div>
</template>

<script>
export default {
    data() {
        return {};
    },
    methods: {
        getImageUrl(imagePath) {
            return `${process.env.VUE_APP_API_URL}${imagePath}`;
        }
    }
};
</script>

<style scoped>
.title {
    display: inline-block;
    padding: 10px;
}

.image {
    width: 250px;
}
</style>
<template>
    <div v-if="digimon">
        <div class="name">
            <img class="element-grid" :alt="`${digimon.type}-${digimon.attribute}`"
                :title="`${digimon.type}-${digimon.attribute}`"
                :src="getImageUrl(`images/element/${digimon.type}-${digimon.attribute}.png`)" />
            <p>{{ digimon.name }}</p>
        </div>
        <p class="number">No. {{ formattedNumber(digimon.number) }}</p>
        <p class="description">{{ digimon.description }}</p>
    </div>
    <div v-else>
        <p>Loading...</p>
    </div>
</template>

<script>

export default {
    props: {
        digimon: {
            type: Object
        }
    },
    methods: {
        formattedNumber(number) {
            if (number < 10) {
                return `00${number}`;
            } else if (number < 100) {
                return `0${number}`;
            } else {
                return number.toString();
            }
        },
        toLowerCase(str) {
            return str.toLowerCase();
        },
        getImageUrl(imagePath) {
            return `${process.env.VUE_APP_API_URL}${imagePath}`;
        }
    }
}

</script>

<style scoped>
.name {
    display: flex;
    align-items: center;
}

.name p {
    font-weight: bold;
    font-size: 3em;
    margin: 0 0 0 10px;
}

.number {
    font-style: normal;
    font-weight: bolder;
    font-size: 1.2em;
    text-align: left;
    margin: 0;
}

.description {
    text-align: justify;
    font-size: 1.1em;
    margin: 5px 0;
}

.element-grid {
    width: 35px;
    height: 35px;
}
</style>
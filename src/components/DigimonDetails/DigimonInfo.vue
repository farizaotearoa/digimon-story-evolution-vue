<template>
    <div v-if="digimon">
        <button @click="$emit('back')" class="arrow-button">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor"
                stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M15 18l-6-6 6-6" />
            </svg>
            <p class="back-button">Back</p>
        </button>
        <div class="info-section">
            <div style="padding:20px;">
                <img :src="getImageUrl(digimon.image)" :alt="digimon.name" :title="digimon.name">
            </div>
            <div class="info-container">
                <div class="info">
                    <p class="key">Stage</p>
                    <div :class="['info-capsule', getStageClass(digimon.stage)]">
                        {{ digimon.stage }}
                    </div>
                </div>
                <div class="info">
                    <p class="key">Type</p>
                    <div :class="['info-capsule', 'type-' + toLowerCase(digimon.type)]">
                        {{ digimon.type }}
                    </div>
                </div>
                <div class="info">
                    <p class="key">Attribute</p>
                    <div :class="['info-capsule', 'attribute-' + toLowerCase(digimon.attribute)]">
                        {{ digimon.attribute }}
                    </div>
                </div>
            </div>
        </div>
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
        getStageClass(stage) {
            switch (stage) {
                case 'Training 1':
                    return 'stage-training1';
                case 'Training 2':
                    return 'stage-training2';
                case 'Rookie':
                    return 'stage-rookie';
                case 'Champion':
                    return 'stage-champion';
                case 'Ultimate':
                    return 'stage-ultimate';
                case 'Mega':
                    return 'stage-mega';
                case 'Ultra':
                case 'Armor':
                    return 'stage-special';
                default:
                    return 'no-stage';
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
.arrow-button {
    background: transparent;
    border: none;
    cursor: pointer;
    display: flex;
    align-items: center;
}

.back-button {
    margin: 0;
    font-size: 20px;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    font-weight: bold;
}

.arrow-button svg {
    width: 25px;
    height: 25px;
}

.info-section {
    padding: 10px;
    text-align: center;
}

.info-section img {
    width: 350px;
    display: inline-block;
}

.info-container {
    display: flex;
    gap: 20px;
    justify-content: center;
}

.info {
    display: flex;
    flex-direction: column;
}

.info p {
    margin: 0
}

.info .key {
    font-weight: 500;
    color: #757575;
}

.info-capsule {
    color: #fff;
    padding: 5px 15px;
    border-radius: 50px;
    font-weight: bold;
    font-size: 0.9rem;
    margin-top: 5px;
}

.stage-training1 {
    background-color: #8ab4d9;
}

.stage-training2 {
    background-color: #274b96;
}

.stage-rookie {
    background-color: #29b376;
}

.stage-champion {
    background-color: #016f3c;
}

.stage-ultimate {
    background-color: #f9a73e;
}

.stage-mega {
    background-color: #bf212f;
}

.stage-special {
    background-color: #FFD700;
    color: #545454;
}

.no-stage {
    background-color: #fff;
    color: #000;
    border: 1px solid #000;
}

.type-free {
    background-color: #545454;
}

.type-data {
    background-color: #545454;
}

.type-vaccine {
    background-color: #545454;
}

.type-virus {
    background-color: #545454;
}

.attribute-neutral {
    background-color: #767676;
}

.attribute-dark {
    background-color: #b26adb;
}

.attribute-light {
    background-color: #fff;
    color: #000;
    border: 1px solid #000;
}

.attribute-fire {
    background-color: #e84b09;
}

.attribute-water {
    background-color: #279cf7;
}

.attribute-plant {
    background-color: #2bbe29;
}

.attribute-electric {
    background-color: #ffd302;
}

.attribute-earth {
    background-color: #96611a;
}

.attribute-wind {
    background-color: #95f6d5;
}
</style>
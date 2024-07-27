<template>
    <div v-if="digimon" class="evolution-container">
        <p class="title">Evolution Chart</p>
        <div class="title-container">
            <p>Digimon to De-Digivolve</p>
            <div class="arrow-gap"></div>
            <div></div>
            <div class="arrow-gap"></div>
            <p>Digimon to Digivolve</p>
        </div>
        <div class="container">
            <div class="digimon-container">
                <ul v-if="dedigivolve.length" class="list-container">
                    <li v-for="digimon in dedigivolve" :key="digimon.number">
                        <p class="digimon-info">{{ digimon.name }}</p>
                        <img :src="getImageUrl(digimon.icon)" class="icon" :alt="digimon.name"
                            :title="digimon.name" @click="selectDigimon(digimon.number)">
                    </li>
                </ul>
                <div v-else>
                    <p>Cannot De-Digivolve further.</p>
                </div>
            </div>
            <svg width="50" height="100">
                <line x1="10" y1="50" x2="40" y2="50" stroke="black" stroke-width="2" />
                <polygon points="30,45 40,50 30,55" fill="black" />
            </svg>
            <div class="digimon-container">
                <p class="digimon-info">{{ digimon.name }}</p>
                <img :src="getImageUrl(digimon.icon)" class="icon" :alt="digimon.name"
                    :title="digimon.name">
            </div>
            <svg width="50" height="100">
                <line x1="10" y1="50" x2="40" y2="50" stroke="black" stroke-width="2" />
                <polygon points="30,45 40,50 30,55" fill="black" />
            </svg>
            <div class="digimon-container">
                <ul v-if="digivolve.length" class="list-container">
                    <li v-for="digimon in digivolve" :key="digimon.next_form">
                        <div class="name-container">
                            <svg class="info-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"
                                @mouseenter="showModal($event, digimon)" @mouseleave="hideModal()">
                                <path
                                    d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 17.93c-4.37 0-7.93-3.56-7.93-7.93S7.63 4.07 12 4.07s7.93 3.56 7.93 7.93-3.56 7.93-7.93 7.93zM11 7h2v2h-2zm1 4c-.55 0-1 .45-1 1v5h2v-5c0-.55-.45-1-1-1z" />
                            </svg>
                            <p class="digimon-info">{{ digimon.next_form_name }}</p>
                            <DigivolveRequirementModal :top="modalPosition.top" :left="modalPosition.left"
                                v-if="activeDigimon && activeDigimon.number === digimon.number">
                                <p class="requirement-title">{{ activeDigimon.next_form_name }} Digivolve Requirement
                                </p>
                                <div class="requirement-container">
                                    <p v-if="activeDigimon.level">Level: {{ activeDigimon.level }}</p>
                                    <p v-if="activeDigimon.atk">ATK: {{ activeDigimon.atk }}</p>
                                    <p v-if="activeDigimon.def">DEF: {{ activeDigimon.def }}</p>
                                    <p v-if="activeDigimon.hp">HP: {{ activeDigimon.hp }}</p>
                                    <p v-if="activeDigimon.int">INT: {{ activeDigimon.int }}</p>
                                    <p v-if="activeDigimon.sp">SP: {{ activeDigimon.sp }}</p>
                                    <p v-if="activeDigimon.spd">SPD: {{ activeDigimon.spd }}</p>
                                    <p v-if="activeDigimon.abi">ABI (Ability): {{ activeDigimon.abi }}</p>
                                    <p v-if="activeDigimon.cam">CAM (Camaraderie): {{ activeDigimon.cam }}%</p>
                                    <p v-if="activeDigimon.additional">Additional: {{ activeDigimon.additional }}</p>
                                </div>
                            </DigivolveRequirementModal>
                        </div>
                        <img :src="getImageUrl(digimon.next_form_icon)" class="icon"
                            :alt="digimon.next_form_name" :title="digimon.next_form_name"
                            @click="selectDigimon(digimon.next_form)">
                    </li>
                </ul>
                <div v-else>
                    <p>Cannot Digivolve further.</p>
                </div>
            </div>
        </div>
    </div>
    <div v-else>
        <p>Loading...</p>
    </div>
</template>

<script>
import axios from '../../axios';
import DigivolveRequirementModal from './DigivolveRequirementModal.vue';

export default {
    components: {
        DigivolveRequirementModal
    },
    props: {
        digimon: {
            type: Object
        }
    },
    data() {
        return {
            digimonEvolutions: [],
            activeDigimon: null,
            modalPosition: {
                top: 0,
                left: 0
            }
        }
    },
    watch: {
        digimon: {
            handler(newValue) {
                if (newValue && newValue.number) {
                    this.fetchDigimonEvolutions();
                }
            },
            immediate: true
        }
    },
    computed: {
        dedigivolve() {
            return this.digimonEvolutions ? this.digimonEvolutions.filter(d => d.next_form === this.digimon.number) : [];
        },
        digivolve() {
            return this.digimonEvolutions ? this.digimonEvolutions.filter(d => d.number === this.digimon.number) : [];
        }
    },
    methods: {
        async fetchDigimonEvolutions() {
            const requestBody = { number: this.digimon.number }
            try {
                const response = await axios.post('/digimon/evolutions', requestBody, {
                    headers: {
                        'Content-Type': 'application/json'
                    }
                });
                this.digimonEvolutions = response.data
            } catch (error) {
                console.error('Error fetching Digimon evolutions:', error);
            }
        },
        formattedNumber(number) {
            if (number < 10) {
                return `00${number}`;
            } else if (number < 100) {
                return `0${number}`;
            } else {
                return number.toString();
            }
        },
        selectDigimon(digimonNumber) {
            this.$emit('digimon-selected', digimonNumber);
        },
        showModal(event, digimon) {
            const rect = event.target.getBoundingClientRect();
            this.modalPosition.top = rect.top + window.scrollY + rect.height;
            this.modalPosition.left = rect.left + window.scrollX;
            this.activeDigimon = digimon;
            this.modalVisible = true;
        },
        hideModal() {
            this.modalVisible = false;
            this.activeDigimon = null;
        },
        getImageUrl(imagePath) {
            return `${process.env.VUE_APP_API_URL}${imagePath}`;
        }
    }
}
</script>

<style scoped>
.container {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.title-container {
    display: flex;
}

.title-container p {
    flex: 1;
    font-weight: bold;
    font-size: 1.4em;
}

.title-container div {
    flex: 1;
}

.arrow-gap {
    width: 50px;
    flex: none !important;
}

.digimon-container {
    flex-direction: column;
    flex: 1;
    padding: 20px;
    border-radius: 8px;
    display: flex;
    justify-content: center;
    align-items: center;
}

.digimon-info {
    margin: 0;
    font-weight: bold;
}

.list-container {
    list-style-type: none;
    padding: 0;
    margin: 0;
}

li .icon {
    cursor: pointer;
}

.icon {
    width: 60px;
}

.evolution-container {
    margin-top: 25px;
    padding-top: 25px;
    border-top: 1px solid #a9a9a9;
}

.title {
    margin: 0 0 20px 0;
    font-weight: bold;
    font-size: 1.5em;
}

.info-icon {
    width: 14px;
    margin-right: 2px;
    padding-bottom: 10px;
}

.name-container {
    display: flex;
    justify-content: center;
}

.requirement-title {
    margin: 0 0 10px 0;
    font-weight: bold;
}

.requirement-container {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    text-align: left;
}

.requirement-container p {
    margin: 0;
}
</style>
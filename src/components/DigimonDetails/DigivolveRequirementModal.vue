<template>
    <div class="hover-modal" :style="{ top: `${top}px`, left: `${left}px` }">
        <div class="modal-content">
            <slot></slot>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        top: {
            type: Number,
            default: 0
        },
        left: {
            type: Number,
            default: 0
        }
    },
    data() {
        return {
            visible: false
        };
    },
    methods: {
        showModal() {
            this.visible = true;
        },
        hideModal() {
            this.visible = false;
        }
    }
};
</script>

<style scoped>
.hover-modal {
    position: absolute;
    background-color: white;
    border: 1px solid #ccc;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    z-index: 10;
    padding: 15px;
}

.modal-content {
    font-size: 0.8em;
}
</style>
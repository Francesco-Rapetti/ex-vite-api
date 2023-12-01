<script>
import AppCard from './AppCard.vue';
import { store } from '../store.js';
export default {
    components: {
        AppCard
    },

    data() {
        return {
            store
        }
    }
}
</script>

<template>
    <div class="wrap d-flex flex-wrap justify-content-center">
        <div class="card-container" v-for="brewery in store.brewery"
            v-show="brewery.brewery_type == store.selectedType || store.selectedType == 'all'">
            <AppCard :id="brewery.id" :title="brewery.name" img="/beer.png" />
        </div>
    </div>
</template>

<style scoped>
.card-container {
    opacity: 0;
    transition: all 0.3s ease-in-out;
}

.asdasd {
    width: 200px;
}

.wrap {
    gap: 20px;
}
</style>
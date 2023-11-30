<script>
import { setTransitionHooks } from "vue";
import { store } from "../store.js";
export default {
    name: 'AppFilter',
    props: ['types', 'optionalFunction'],
    data() {
        return {
            // types: ['ciao', 'hello', 'come', 'stai', 'asdasd', 'asdasd'],
            filterValue: 'default filter',
            store,
        }
    },
    methods: {
        clickDebug() {
            console.log(this.filterValue);
            console.log(this.store.selectedType);
            console.log(store.selectedType);
        }
    }
}
</script>

<template>
    <div class="d-flex align-items-center justify-content-center m-5">
        <select class="form-select" aria-label="Default select example" @click="filterValue = $event.target.value">
            <option selected value="null">Filter by type</option>
            <option v-for="item in types" :value="item">{{ item }}</option>
        </select>
        <button class="btn ms-3"
            @click="store.selectedType = filterValue; if (optionalFunction) optionalFunction(); clickDebug()">Filter</button>
    </div>
</template>

<style scoped>
select {
    width: 200px;
    background-color: transparent;
    border: 4px solid white;
    cursor: pointer;
}

.form-select:focus {
    outline: none !important;
    border-color: white !important;
    -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, .075), 0 0 8px rgba(0, 0, 0, 0.1);
    box-shadow: inset 0 1px 1px rgba(0, 0, 0, .075), 0 0 8px rgba(0, 0, 0, 0.1);
}

.btn {
    border: 4px solid white;
}

.btn:hover {
    background-color: #fbb533;
}

.btn:active {
    border-color: transparent !important;
}
</style>
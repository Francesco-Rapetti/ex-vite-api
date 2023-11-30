<script>
import AppFilter from './components/AppFilter.vue';
import { store } from './store.js';
import axios from 'axios';
export default {
	components: {
		AppFilter
	},

	data() {
		return {
			store
		}
	},

	methods: {
		/**
		 * Retrieves the types of breweries from the API and adds them to the store's `breweryTypes` array.
		 *
		 * @param {type} paramName - description of parameter
		 * @return {type} description of return value
		 */
		getTypes() {
			// Make a GET request to the API
			axios.get(this.store.apiUrl).then(res => {
				// Loop through the response data
				res.data.forEach(obj => {
					// Check if the brewery type is not already in the store's `breweryTypes` array
					if (!this.store.breweryTypes.includes(obj.brewery_type)) {
						// Add the brewery type to the store's `breweryTypes` array
						this.store.breweryTypes.push(obj.brewery_type);
					}
				});
			});

			// Log the updated `breweryTypes` array
			console.log(this.store.breweryTypes);
		}
	},

	mounted() {
		this.getTypes();
	}
}
</script>

<template>
	<div class="wrapper">
		<div id="glass-container" class="glass">
			<AppFilter :types="store.breweryTypes" />
		</div>
	</div>
</template>

<style scoped>
:root {
	--glass-container-height: auto;
	--glass-container-width: auto;
	--glass-container-top: auto;
	--glass-container-bottom: auto;
}

.wrapper {
	height: 100vh;
	display: flex;
	justify-content: center;
	align-items: center;
}

#glass-container {
	/* height: calc(100vh - 40px); */
	/* width: calc(100% - 40px); */
	height: var(--glass-container-height);
	width: var(--glass-container-width);
	max-width: 1600px;
	position: relative;
	top: var(--glass-container-top);
	bottom: var(--glass-container-bottom);
	margin: 0 auto;
	transition: all 0.3s ease-in-out;
	background: rgba(255, 255, 255, 0.47);
	border-radius: 16px;
	box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
	backdrop-filter: blur(7px);
	-webkit-backdrop-filter: blur(7px);
}
</style>

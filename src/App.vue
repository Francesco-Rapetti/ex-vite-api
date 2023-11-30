<script>
import AppFilter from './components/AppFilter.vue';
import AppContent from './components/AppContent.vue';
import { store } from './store.js';
import axios from 'axios';
export default {
	components: {
		AppFilter,
		AppContent
	},

	data() {
		return {
			store
		}
	},

	methods: {

		/**
		 * Retrieves a list of breweries from the API and updates the `brewery` array in the store.
		 *
		 * @return {void} 
		 */
		getBreweries() {
			// Make a GET request to the API
			axios.get(this.store.apiUrl).then(res => {
				// Loop through the response data
				res.data.forEach(obj => {
					// Add the brewery object to the `brewery` array in the store
					this.store.brewery.push(obj);
					// Call the `getTypes` method to get the types of the brewery
					this.getTypes(obj);
				});
			});

			// Log the updated `breweryTypes` array
			console.log(this.store.breweryTypes);
			console.log(this.store.brewery);
		},

		/**
		 * Adds the brewery type to the store's `breweryTypes` array if it is not already present.
		 *
		 * @param {Object} obj - The object containing the `brewery_type` property.
		 */
		getTypes(obj) {
			// Check if the brewery type is not already in the store's `breweryTypes` array
			if (!this.store.breweryTypes.includes(obj.brewery_type)) {
				// Add the brewery type to the store's `breweryTypes` array
				this.store.breweryTypes.push(obj.brewery_type);
			}
		},

		debugFunction() {
			console.log("triggered");
		},

		displayBreweries() {
			const container = document.getElementById('glass-container');
			if (this.store.selectedType !== null) {
				container.style.setProperty('height', 'calc(100vh - 40px)');
				container.style.setProperty('width', 'calc(100% - 40px');
			}
		}
	},

	mounted() {
		this.getBreweries();
	}
}
</script>

<template>
	<div class="wrapper">
		<div id="glass-container" class="glass">
			<AppFilter :types="store.breweryTypes" :optional-function="displayBreweries" />
			<AppContent />
		</div>
	</div>
</template>

<style scoped>
:root {
	--glass-container-height: 170px;
	--glass-container-width: 380px;
	--glass-container-top: 0;
	--glass-container-bottom: 0;
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
	height: 140px;
	width: 380px;
	max-width: 1600px;
	/* position: relative; */
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

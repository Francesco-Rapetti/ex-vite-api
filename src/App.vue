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

		/**
		 * This function is used for debugging purposes. It logs the string "triggered" to the console.
		 *
		 * @param {type} paramName - This function does not accept any parameters.
		 * @return {type} This function does not return anything.
		 */
		debugFunction() {
			console.log("triggered");
		},

		/**
		 * Displays the breweries in the glass container.
		 *
		 * @return {void} This function does not return anything.
		 */
		async displayBreweries() {
			// Delay function to pause execution
			const delay = ms => new Promise(res => setTimeout(res, ms));

			// Get the container element and card elements
			const container = document.getElementById('glass-container');
			const cards = document.querySelectorAll('.card-container');

			// Animate the container size
			container.style.transition = 'height 0.3s ease-in-out, width 0.3s ease-in-out';
			container.style.setProperty('height', 'calc(100vh - 40px)');
			container.style.setProperty('width', 'calc(100% - 40px');

			// Wait for 300ms before continuing
			await delay(300);

			// Disable transition and make cards visible
			container.style.transition = 'none';
			cards.forEach(card => {
				card.style.opacity = '1';
			});
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
			<div class="content">
				<AppContent />
			</div>
		</div>
	</div>
</template>

<style scoped>
.wrapper {
	height: 100vh;
	display: flex;
	justify-content: center;
	align-items: center;
}

#glass-container {
	height: 140px;
	width: 380px;
	max-width: 1600px;
	margin: 0 auto;

	background: rgba(255, 255, 255, 0.47);
	border-radius: 16px;
	box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
	backdrop-filter: blur(7px);
	-webkit-backdrop-filter: blur(7px);
}

.content {
	height: calc(100% - 140px);
	overflow: auto;
	padding: 100px;
}
</style>

<script>
export default {
    name: 'AppCard',
    props: ['title', 'img', 'id', 'type', 'address', 'phone', 'website', 'mapsLink'],

    data() {
        return {
            cardActive: null
        }
    },

    methods: {
        /**
         * Generates a random color and sets it as the background color for all image containers.
         */
        randomColorPicsBg() {
            // Generate a random hexadecimal color
            let randomColor = Math.floor(Math.random() * 16777215).toString(16).padStart(6, 0);

            // Get all image containers
            const imgContainers = document.querySelectorAll('.img-container');

            // Set the background color for each image container
            imgContainers.forEach(imgContainer => {
                imgContainer.style.backgroundColor = "#" + randomColor;

                // Generate a new random color for the next image container
                randomColor = Math.floor(Math.random() * 16777215).toString(16).padStart(6, 0);
            });
        },

        /**
         * Function to show more information for a given ID.
         * @param {string} id - The ID of the element to show more information for.
         */
        async moreInfo(id) {
            const delay = ms => new Promise(res => setTimeout(res, ms));
            // Get the card element with the given ID
            const bigCard = document.getElementById(`bigCard${id}`);
            const card = document.getElementById(`card${id}`);

            // Reset card style if it's already active
            if (this.cardActive) {
                // this.resetCardStyle(bigCard);
            } else {
                // Set card as active and modify its style
                console.log(`bigCard${id}`);
                this.cardActive = bigCard;
                bigCard.style.opacity = '1';
                bigCard.style.width = `400px`;
                bigCard.style.height = `400px`;
                bigCard.style.top = '-50%';
                bigCard.style.left = '-50%';
                card.style.cursor = 'default';

                // Set other card containers to lower z-index and hide them
                document.querySelectorAll('.card-container').forEach(card => {
                    if (card.firstChild.id.substring(4) !== this.cardActive.id.substring(7)) {
                        card.style.zIndex = '-1';
                        // console.log('check')
                        // console.log(card.firstChild.id)
                    }
                })

                // Wait for 300 milliseconds
                await delay(300);

                // Log the child nodes of the bigCard element
                // console.log(bigCard.childNodes);

                // Iterate over each child node of the bigCard element
                bigCard.childNodes.forEach(async child => {
                    child.style.display = 'block';

                    await delay(10);

                    child.style.opacity = '1';
                });
            }

            // console.log(this.cardActive);
        },

        /**
         * Resets the style of a card element.
         * @param {HTMLElement} bigCard - The card element to reset.
         */
        async resetCardStyle(bigCard) {
            const delay = ms => new Promise(res => setTimeout(res, ms));

            // Fade out and hide each child node of the bigCard element
            bigCard.childNodes.forEach(async child => {
                child.style.opacity = '0';
                await delay(300);
                child.style.display = 'none';
            });

            await delay(300);

            // Resetting cardActive to null
            this.cardActive = null;

            // Setting the opacity, width, height, top, and left properties of the bigCard element
            bigCard.style.opacity = '0';
            bigCard.style.width = '200px';
            bigCard.style.height = '200px';
            bigCard.style.top = '0';
            bigCard.style.left = '0';

            // Set the z-index of all card containers to 0
            document.querySelectorAll('.card-container').forEach(card => {
                card.style.zIndex = '0';
                card.firstChild.style.cursor = 'pointer';
            });
        },

        getImagePath(img) {
            return new URL(`../assets/img/${img}`, import.meta.url).href
        }
    },

    mounted() {
        this.randomColorPicsBg();
        document.addEventListener('click', (event) => {
            if (this.cardActive && !this.cardActive.contains(event.target)) {
                this.resetCardStyle(this.cardActive);
            }
        })
    }
}
</script>

<template>
    <div :id="'card' + id" class="card-element" @click="moreInfo(id)">
        <div class="img-container rounded-circle overflow-hidden">
            <img class="w-75" :src="getImagePath(img)" alt="">
        </div>
        <div :id="'bigCard' + id" class="full-screen-card p-5">
            <h4 class="mb-4">{{ title }}</h4>
            <ul class="fa-ul">
                <!-- Brewery type -->
                <li class="text-start mb-3">
                    <i class="fa-li fa-solid fa-beer-mug-empty"></i>
                    {{ type }}
                </li>
                <!-- Brewery address -->
                <li class="text-start mb-3">
                    <i class="fa-li fa-solid fa-location-dot"></i>
                    {{ address }}
                </li>
                <!-- Brewery phone -->
                <li class="text-start mb-3">
                    <i class="fa-li fa-solid fa-phone"></i>
                    {{ phone ? phone : 'N/A' }}
                </li>
                <!-- Brewery website -->
                <li class="text-start mb-3">
                    <i class="fa-li fa-solid fa-globe"></i>
                    <a :href="website" target="_blank">
                        {{ website ? website : 'N/A' }}
                    </a>
                </li>
                <!-- Brewery website -->
                <li class="text-start mb-3">
                    <i class="fa-li fa-solid fa-map-location-dot"></i>
                    <a :href="mapsLink" target="_blank">
                        Google Maps
                    </a>
                </li>
            </ul>
        </div>
        <!-- TODO: google maps link (https://www.google.com/maps/place/ + latitude + longitude) -->
        <p class="mt-3">{{ title }}</p>
    </div>
</template>

<style scoped>
.card-element {
    position: relative;
    cursor: pointer;
    text-align: center;
    width: 200px;
    height: 200px;
    padding: 1rem;
    color: white;
    background: rgba(0, 0, 0, 0.79);
    /* debug color */
    /* background-color: red; */
    border-radius: 16px;
    transition: height 0.3s ease-in-out, width 0.3s ease-in-out;
    box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
    backdrop-filter: blur(13.6px);
    -webkit-backdrop-filter: blur(13.6px);
    z-index: 0;
}

.full-screen-card {
    position: absolute;
    top: 0;
    left: 0;
    width: inherit;
    height: inherit;
    border-radius: inherit;
    background-color: inherit;
    box-shadow: inherit;
    backdrop-filter: inherit;
    -webkit-backdrop-filter: inherit;
    opacity: 0;
    transition: all 0.3s ease-in-out;
    z-index: 1 !important;
}

.full-screen-card>* {
    display: none;
    opacity: 0;
    transition: opacity 0.3s ease-in-out;
}

.img-container {
    width: 100px;
    height: 100px;
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: center;
}
</style>
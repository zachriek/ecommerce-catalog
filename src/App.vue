<template>
	<AppProduct :product="currentProduct" :loading="loading" @fetch-product="fetchProducts" />
</template>

<script>
import axios from 'axios';
import AppProduct from './components/AppProduct.vue';

const API_URL = 'https://fakestoreapi.com/products/';

export default {
	components: {
		AppProduct,
	},
	data() {
		return {
			currentProduct: null,
			currentIndex: 1,
			loading: true,
		};
	},
	methods: {
		async fetchProducts() {
			this.loading = true;
			try {
				const response = await axios.get(API_URL + this.currentIndex);
				const productData = response.data;

				if (productData.category === "men's clothing" || productData.category === "women's clothing") {
					this.currentProduct = productData;
				} else {
					this.currentProduct = { title: 'This product is unavailable to show' };
				}

				this.currentIndex = this.currentIndex === 20 ? 1 : this.currentIndex + 1;
			} catch (error) {
				console.error('Error fetching product:', error);
			} finally {
				this.loading = false;
			}
		},
	},
	mounted() {
		this.fetchProducts();
	},
};
</script>

<style src="./assets/css/style.css"></style>

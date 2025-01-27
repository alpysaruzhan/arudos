<template>
    <div v-if="loading" class="loading">Loading product details...</div>
    <div v-if="errorMessage" class="error">{{ errorMessage }}</div>
    <div v-if="product" class="product-detail">
        <img :src="product.imageUrl" alt="Product Image" class="product-image" />
        <h2>{{ product.title }}</h2>
        <p>Artist: {{ product.artist }}</p>
        <p>Genre: {{ product.genre }}</p>
        <p>Price: ${{ product.price }}</p>
        <p>{{ product.description }}</p>
        <p>In Stock: {{ product.stockQuantity }}</p>

        <label for="quantity">Quantity:</label>
        <input type="number" id="quantity" v-model="quantity" min="1" :max="product.stockQuantity" />

        <button @click="addToCart">Add to Cart</button>
    </div>
</template>

<script>
/*eslint-disable*/
import axios from 'axios';

export default {
    data() {
        return {
            product: null,
            quantity: 1,
            loading: true,
            errorMessage: ''
        };
    },
    created() {
        this.fetchProductDetails();
    },
    methods: {
        async fetchProductDetails() {
            const productId = this.$route.params.productId;
            try {
                const response = await axios.get(`http://localhost:8080/api/products/${productId}`);
                this.product = response.data;
            } catch (error) {
                this.errorMessage = 'Failed to load product details. Please try again later.';
                console.error(error);
            } finally {
                this.loading = false;
            }
        },
        async addToCart() {
            const productId = this.product.productId;
            try {
                const response = await axios.post(
                    `http://localhost:8080/api/cart/add?productId=${productId}&quantity=${this.quantity}`, // Send productId and quantity as query parameters
                    {},
                    { headers: { Authorization: `Bearer ${localStorage.getItem('jwt')}` } }
                );
                alert('Product added to cart successfully');
            } catch (error) {
                alert('Failed to add product to cart');
                console.error(error);
            }
        }

    }
};
</script>

<style scoped>
.product-detail {
    padding: 20px;
    text-align: center;
}

.product-image {
    max-width: 100%;
    height: auto;
}

button {
    margin-top: 20px;
    padding: 10px 20px;
    background-color: #28a745;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background-color: #218838;
}
</style>

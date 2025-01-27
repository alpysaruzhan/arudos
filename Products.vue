<template>
    <div class="products-container">
        <h2>Our Products</h2>
        <div v-if="loading">Loading products...</div>
        <div v-if="errorMessage" class="error">{{ errorMessage }}</div>
        <div v-if="products.length > 0" class="products-list">
            <div v-for="product in products" :key="product.productId" class="product-card">
                <img :src="product.imageUrl" alt="Product Image" class="product-image"/>
                <h3>{{ product.title }}</h3>
                <p>Artist: {{ product.artist }}</p>
                <p>Genre: {{ product.genre }}</p>
                <p> ${{ product.price }}</p>
                <p>In Stock: {{ product.stockQuantity }}</p>
                <p>{{ product.description }}</p>
                <router-link :to="'/product/' + product.productId" class="view-details-btn">View Details</router-link>
            </div>
        </div>

        <div class="de">
            <div class="pr">
            <img src="./image155.png" alt="Product Image" class="product-image"/>
            <h3>Дос-Мукасан</h3>
            <p> $155</p>
            </div>
            <div class="pr">
            <img src="./image155.png" alt="Product Image" class="product-image"/>
            <h3>Дос-Мукасан</h3>
            <p> $155</p>
            </div>
            <div class="pr">
            <img src="./image155.png" alt="Product Image" class="product-image"/>
            <h3>Дос-Мукасан</h3>
            <p> $155</p>
            </div>
        <div class="pr">
            <img src="./image155.png" alt="Product Image" class="product-image"/>
            <h3>Дос-Мукасан</h3>
            <p> $155</p>
        </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            products: [],
            loading: true,
            errorMessage: ''
        };
    },
    created() {
        this.fetchProducts();
    },
    methods: {
        async fetchProducts() {
            try {
                const response = await axios.get('http://localhost:8080/api/products');
                this.products = response.data;
            } catch (error) {
                this.errorMessage = 'Failed to load products. Please try again later.';
                console.error(error);
            } finally {
                this.loading = false;
            }
        }
    }
};
</script>

<style scoped>
.products-container {
    padding: 20px;
}

.pr{
    text-align: center;
}

.error {
    color: red;
    margin-top: 10px;
}

h3{
    font-weight: 400;
}

.pr{
    margin: 10px;
    max-width: 310px;
}

.de{
    display: flex;
    align-items: center;
    justify-content: center;
}

.products-list {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
}

.product-card {
    border: 1px solid #ccc;
    padding: 15px;
    width: 250px;
    text-align: center;
}

.product-image {
    max-width: 100%;
    height: auto;
    width: 310px;
}

.view-details-btn {
    display: inline-block;
    margin-top: 10px;
    padding: 10px;
    background-color: #28a745;
    color: white;
    text-decoration: none;
    border-radius: 5px;
}

.view-details-btn:hover {
    background-color: #218838;
}
</style>

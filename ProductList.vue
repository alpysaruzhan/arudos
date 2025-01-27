<template>
    <div>
        <h1>Product List</h1>

        <div v-if="errorMessage" class="error">{{ errorMessage }}</div>

        <ul v-if="products.length > 0">
            <li v-for="product in products" :key="product.productId">
                <h3>{{ product.title }} - ${{ product.price }}</h3>
                <p>{{ product.description }}</p>
                <button @click="deleteProduct(product.productId)">Delete</button>
                <button @click="editProduct(product)">Edit</button>
            </li>
        </ul>

        <h2>Add / Edit Product</h2>
        <form @submit.prevent="saveProduct">
            <input v-model="form.title" placeholder="Title" required />
            <input v-model="form.artist" placeholder="Artist" required />
            <input v-model="form.genre" placeholder="Genre" required />
            <input v-model.number="form.releaseYear" placeholder="Release Year" required />
            <input v-model.number="form.price" placeholder="Price" required />
            <input v-model.number="form.stockQuantity" placeholder="Stock Quantity" required />
            <textarea v-model="form.description" placeholder="Description" required></textarea>
            <input v-model="form.imageUrl" placeholder="Image URL" />
            <button type="submit">Save</button>
        </form>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            products: [],
            errorMessage: '',
            form: {
                productId: null,
                title: '',
                artist: '',
                genre: '',
                releaseYear: '',
                price: '',
                stockQuantity: '',
                description: '',
                imageUrl: ''
            }
        };
    },
    created() {
        // this.checkAdminRole();  // Check the user's role when the component is created
        this.fetchProducts();
    },
    methods: {
        getAuthHeader() {
            const token = localStorage.getItem('jwt');
            return { Authorization: `Bearer ${token}` };
        },
        async fetchProducts() {
            try {
                const response = await axios.get('http://localhost:8080/api/products', {
                    headers: this.getAuthHeader()
                });
                this.products = response.data;
            } catch (error) {
                this.errorMessage = 'Failed to load products.';
            }
        },
        async saveProduct() {
            try {
                if (this.form.productId) {
                    await axios.put(`http://localhost:8080/api/products/${this.form.productId}`, this.form, {
                        headers: this.getAuthHeader()
                    });
                } else {
                    await axios.post('http://localhost:8080/api/products', this.form, {
                        headers: this.getAuthHeader()
                    });
                }
                this.fetchProducts();
                this.resetForm();
            } catch (error) {
                this.errorMessage = 'Failed to save product.';
            }
        },
        editProduct(product) {
            this.form = { ...product };
        },
        resetForm() {
            this.form = {
                productId: null,
                title: '',
                artist: '',
                genre: '',
                releaseYear: '',
                price: '',
                stockQuantity: '',
                description: '',
                imageUrl: ''
            };
        },
        async deleteProduct(id) {
            try {
                await axios.delete(`http://localhost:8080/api/products/${id}`, {
                    headers: this.getAuthHeader()
                });
                this.fetchProducts();
            } catch (error) {
                this.errorMessage = 'Failed to delete product.';
            }
        },
        // checkAdminRole() {
        //     const token = localStorage.getItem('jwt');
        //     if (token) {
        //         try {
        //             const decoded = JSON.parse(atob(token.split('.')[1]));  // Decode the JWT token
        //             // Check if the decoded object has roles and if 'ADMIN' is included in the roles
        //             if (decoded && decoded.roles && decoded.roles.includes("ROLE_ADMIN")) {
        //                 // User is an admin, proceed to load the product list
        //                 return;
        //             }
        //         } catch (error) {
        //             console.error('Error decoding token:', error);  // Log any errors during decoding
        //         }
        //     }
        //     // If the token is not present, or the user is not an admin, redirect to the main page
        //     this.$router.push("/main");
        // }



    }
};
</script>

<style scoped>
.error {
    color: red;
}
</style>
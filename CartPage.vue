<template>
    <div class="cart-container">
        <h1>Your Cart</h1>

        <table v-if="cartItems.length > 0">
            <thead>
                <tr>
                    <th>Product Name</th>
                    <th>Quantity</th>
                    <th>Total Price</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="item in cartItems" :key="item.cartItemId">
                    <td>{{ item.productName }}</td>
                    <td>
                        <button @click="decreaseQuantity(item.cartItemId)" :disabled="item.quantity <= 1">
                            -
                        </button>
                        {{ item.quantity }}
                        <button @click="increaseQuantity(item.cartItemId)">
                            +
                        </button>
                    </td>
                    <td>{{ item.totalPrice }}</td>
                    <td>
                        <button @click="deleteCartItem(item.cartItemId)" class="delete-button">Remove</button>
                    </td>
                </tr>
            </tbody>
        </table>

        <p v-else>Your cart is empty.</p>
    </div>
</template>

<script>
 /* eslint-disable */
import axios from 'axios';

export default {
    name: 'CartPage',
    data() {
        return {
            cartItems: [] // To store cart items
        };
    },
    created() {
        this.fetchCartItems();
    },
    methods: {
        async fetchCartItems() {
            try {
                const token = localStorage.getItem('jwt');
                const response = await axios.get('http://localhost:8080/api/cart/view', {
                    headers: { Authorization: `Bearer ${token}` }
                });
                this.cartItems = response.data.map(item => ({
                    ...item,
                }));
            } catch (error) {
                console.error('Error fetching cart items:', error);
            }
        },

        async decreaseQuantity(cartItemId) {
            try {
                const token = localStorage.getItem('jwt');
                const response = await axios.put(
                    `http://localhost:8080/api/cart/decrease/${cartItemId}`,
                    {},
                    {
                        headers: { Authorization: `Bearer ${token}` }
                    }
                );

                // Update quantity and total price directly in cartItems
                const itemIndex = this.cartItems.findIndex(item => item.cartItemId === cartItemId);
                if (itemIndex !== -1) {
                    this.cartItems[itemIndex].quantity -= 1;
                    this.cartItems[itemIndex].totalPrice =
                        this.cartItems[itemIndex].quantity * this.cartItems[itemIndex].productPrice; // Assuming productPrice is available
                }
            } catch (error) {
                console.error('Error decreasing quantity:', error);
            }
        },
        async increaseQuantity(cartItemId) {
            try {
                const token = localStorage.getItem('jwt');
                const response = await axios.put(
                    `http://localhost:8080/api/cart/increase/${cartItemId}`,
                    {},
                    {
                        headers: { Authorization: `Bearer ${token}` }
                    }
                );
                // Update quantity and total price directly in cartItems
                const itemIndex = this.cartItems.findIndex(item => item.cartItemId === cartItemId);
                if (itemIndex !== -1) {
                    this.cartItems[itemIndex].quantity += 1;
                    this.cartItems[itemIndex].totalPrice =
                        this.cartItems[itemIndex].quantity * this.cartItems[itemIndex].productPrice; // Assuming productPrice is available
                }
            } catch (error) {
                console.error('Error increasing quantity:', error);
            }
        },



        async deleteCartItem(cartItemId) {
            try {
                const token = localStorage.getItem('jwt');
                await axios.delete(`http://localhost:8080/api/cart/remove/${cartItemId}`, {
                    headers: { Authorization: `Bearer ${token}` }
                });

                // Update the cart by removing the item locally
                this.cartItems = this.cartItems.filter(item => item.cartItemId !== cartItemId);
                alert('Item successfully removed from the cart.');
            } catch (error) {
                console.error('Error deleting cart item:', error);
                alert('Failed to delete the item. Please try again.');
            }
        }
    }
};
</script>

<style scoped>
.cart-container {
    padding: 20px;
    max-width: 800px;
    margin: 0 auto;
}

h1 {
    text-align: center;
}

table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
}

table,
th,
td {
    border: 1px solid #ddd;
}

th,
td {
    padding: 8px;
    text-align: left;
}

td {
    text-align: center;
}

p {
    text-align: center;
    font-size: 18px;
    color: #888;
}

button {
    padding: 5px 10px;
    cursor: pointer;
    border-radius: 4px;
}

.delete-button {
    background-color: #ff4d4d;
    color: white;
    border: none;
}

.delete-button:hover {
    background-color: #e63939;
}

button:disabled {
    background-color: #ccc;
    cursor: not-allowed;
}
</style>

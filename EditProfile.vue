<template>
    <div class="edit-profile-container">
        <h2>Edit Profile</h2>

        <div v-if="loading">Loading user information...</div>
        
        <form v-else @submit.prevent="updateUserInfo">
            <div>
                <label for="name">Name:</label>
                <input type="text" v-model="name" />
            </div>

            <div>
                <label for="email">Email:</label>
                <input type="email" v-model="email" />
            </div>

            <div>
                <label for="password">Password:</label>
                <input type="password" v-model="password" placeholder="Enter new password" />
            </div>

            <div>
                <label for="phone">Phone:</label>
                <input type="text" v-model="phone" />
            </div>

            <div>
                <label for="address">Address:</label>
                <input type="text" v-model="address" />
            </div>

            <button type="submit">Save Changes</button>
            <p v-if="successMessage" class="success">{{ successMessage }}</p>
            <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
        </form>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            name: '',
            email: '',
            password: '',
            phone: '',
            address: '',
            loading: true,
            successMessage: '',
            errorMessage: ''
        };
    },
    created() {
        this.getUserInfo();
    },
    methods: {
        async getUserInfo() {
            const token = localStorage.getItem('jwt');
            // if (!token) {
            //     this.$router.push('/login');
            //     return;
            // }

            try {
                const response = await axios.get('http://localhost:8080/api/users/me', {
                    headers: { Authorization: `Bearer ${token}` }
                });
                const userData = response.data;

                this.name = userData.name;
                this.email = userData.email;
                this.phone = userData.phone;
                this.address = userData.address;
            } catch (error) {
                console.error('Failed to fetch user info:', error);
                this.errorMessage = 'Failed to load user information.';
            } finally {
                this.loading = false;
            }
        },
        async updateUserInfo() {
            const token = localStorage.getItem('jwt');
            try {
                await axios.put(
                    'http://localhost:8080/api/users/me',
                    {
                        name: this.name,
                        email: this.email,
                        passwordHash: this.password ? this.password : null,
                        phone: this.phone,
                        address: this.address,
                    },
                    { headers: { Authorization: `Bearer ${token}` } }
                );

                this.successMessage = 'Profile updated successfully!';
                this.errorMessage = '';
            } catch (error) {
                console.error('Failed to update user info:', error);
                this.errorMessage = 'Failed to update user information.';
            }
        }
    }
};
</script>

<style scoped>
.edit-profile-container {
    max-width: 600px;
    margin: 10px auto;
    padding: 20px;
    font-weight: 400;
}

.edit-profile-container h2 {
    text-align: center;
    margin-bottom: 20px;
}

form div {
    margin-bottom: 15px;
    padding-right: 15px;
}

h2{
    font-weight: 400;
}

form label {
    display: block;
    margin-bottom: 5px;
}

form input {
    width: 100%;
    padding: 10px;
    font-size: 14px;
    border: none;
    background-color: #D9D9D9;
    height: 30px;

}

form input:focus {
    outline: none;
}

button[type="submit"] {
    width: 100%;
    padding: 10px;
    background-color: #AB8C52;
    color: white;
    border: none;
    font-size: 16px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

button[type="submit"]:hover {
    background-color: #0056b3;
}

.success {
    margin-top: 10px;
    color: #28a745;
    text-align: center;
}

.error {
    margin-top: 10px;
    color: #dc3545;
    text-align: center;
}
</style>

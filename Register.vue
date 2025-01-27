<template>
    <div class="register-container">
        <h2>Register</h2>
        <form @submit.prevent="registerUser">

            <div class="forms">
                <label for="email">Email:</label>
                <input class="inp"  type="email" v-model="email" required />
            </div>

            <div class="forms">
                <label for="password">Password:</label>
                <input class="inp" type="password" v-model="password" required />
            </div>

            <div class="dai">
            <button class="bts" type="submit">Register</button>
            <p v-if="successMessage" class="success">{{ successMessage }}</p>
            <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
            <p>
                Have an account? 
                <router-link to="/login">Log in</router-link>
            </p>
            </div>
        </form>
    </div>
</template>

<script>
import axios from 'axios';
import { useRouter } from 'vue-router';

export default {
    data() {
        return {
            email: '',
            password: '',
            successMessage: '',
            errorMessage: '',
        };
    },
    setup() {
        const router = useRouter();
        return { router };
    },
    methods: {
        async registerUser() {
            try {
                await axios.post('http://localhost:8080/api/register', {
                    email: this.email,
                    passwordHash: this.password,
                });

                this.successMessage = 'User registered successfully!';
                this.errorMessage = '';
                this.email = '';
                this.password = '';

                // Redirect to the login page after successful registration
                this.router.push('/login');
            } catch (error) {
                if (error.response && error.response.status === 400) {
                    this.errorMessage = 'Email already exists. Please use a different email.';
                } else {
                    this.errorMessage = 'Registration failed. Please try again later.';
                }
                this.successMessage = '';
            }
        },
    },
};
</script>

<style scoped>
.register-container {
    max-width: 400px;
    margin: auto;
}
.success {
    color: green;
}
.error {
    color: red;
}
.inp{
    border: none;
    width: 440px;
    height: 50px;
    background-color: #D9D9D9;
    margin-top: 15px;
    margin-bottom: 5px;
}

.forms{
    text-align: left;
}

h2{
    font-weight: 400;
}

.bts{
    border: none;
    background-color: #AB8C52;
    width: 100px;
    height: 40px;
    font-size: 20px;
    color: white;
    align-items: start;
    margin-right: 10px;
}

.dai{
    align-items: start;
    display: flex;
    margin-top: 15px;
}
</style>

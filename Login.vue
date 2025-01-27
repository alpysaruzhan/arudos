<template>
    <div class="login-container">
        <h2>Войти</h2>
        <form @submit.prevent="loginUser">
            <div class="forms">
                <label for="email">Email:</label>
                <input class="inp"  type="email" v-model="email" required />
            </div>

            <div class="forms">
                <label for="password">Password:</label>
                <input class="inp" type="password" v-model="password" required />
            </div>

            <div class="dai">
                <button class="bts" type="submit">Войти</button>
            <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
            <p>
                Haven't got an account? 
                <router-link to="/register">Create</router-link>
            </p>
            </div>
        </form>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            email: '',
            password: '',
            errorMessage: '',
        };
    },
    methods: {
        async loginUser() {
            try {
                const response = await axios.post('http://localhost:8080/api/login', {
                    email: this.email,
                    password: this.password,
                });

                const token = response.data.token;
                localStorage.setItem('jwt', token); // Save token locally

                this.$router.push('/main'); // Navigate to main page
                this.errorMessage = ''; // Clear error message
            } catch (error) {
                if (error.response && error.response.status === 401) {
                    this.errorMessage = 'Invalid login credentials. Please try again.';
                } else {
                    this.errorMessage = 'Login failed. Please try again later.';
                }
            }
        },
    },
};
</script>

<style scoped>
.login-container {
    max-width: 400px;
    margin: auto;
    text-align: center;
    text-align: center;
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
    text-align: left;
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

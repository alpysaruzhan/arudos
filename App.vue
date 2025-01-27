<template>
  <div class="promo-banner">
      <p>Получите скидку по промокоду “START”</p>
    </div>
    <header class="header">
      <div class="logo">
        <img src="./components/logoo.png" alt="Logo" />
      </div>
      <div class="profile-section">
        <nav class="nav-menu">
          <ul>
            <li><a href="#">Каталог</a></li>
          </ul>
        </nav>
        <button @click="editProfile" class="profile-btn">
          <svg
            width="24"
            height="24"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            viewBox="0 0 24 24"
            class="feather feather-user"
          >
            <path
              d="M12 12c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zM12 14c-3.31 0-6 2.69-6 6v2h12v-2c0-3.31-2.69-6-6-6z"
            />
          </svg>
        </button>
        <button @click="logout" class="logout-btn">
          <svg
            width="24"
            height="24"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            stroke="#000"
            stroke-width="2"
            viewBox="0 0 24 24"
            class="feather feather-log-out black-icon"
          >
            <path
              d="M17 11l4-4m0 0l-4-4m4 4H3m14 4H7a2 2 0 0 0-2 2v3a2 2 0 0 0 2 2h8a2 2 0 0 0 2-2v-3a2 2 0 0 0-2-2z"
            />
          </svg>
        </button>
      </div>
    </header>

    <router-view /> 


</template>

<script>
export default {
  name: 'App', 
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@100;400;700;900&display=swap'); 

body {
  font-family: 'Montserrat', sans-serif; 
  margin: 0;
  padding: 0;
  background-color: #F2F1EC;
}

.promo-banner {
  background-color: #333;
  color: #ffffff;
  text-align: center;
  padding: 10px 0;
  font-size: 15px;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
  background-color: #f2f1ec;
  margin-left: 80px;
  margin-right: 80px;
}

.logo img {
  height: 30px;
}

.nav-menu ul {
  list-style: none;
  display: flex;
  gap: 20px;
  margin-right: 50px;
}
.nav-menu ul li a {
  text-decoration: none;
  color: #313034;
  font-size: 1rem;
}

.nav-menu ul li a:hover {
  color: #363636;
}

.profile-section {
  display: flex;
  align-items: center;
  gap: 10px;
}

.profile-btn {
  background-color: transparent;
  border: none;
  color: #313034;
  cursor: pointer;
  margin-right: 10px;
}

.profile-btn svg {
  width: 24px;
  height: 24px;

}


.btn {
  background-color: #4caf50;
  color: #313034;
  border: none;
  padding: 10px 20px;
  margin: 10px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.3s;
}

.btn:hover {
  background-color: #45a049;
}

.logout-btn {
  background: transparent;
  border: none;
  color: #313034;
}

.white-icon {
  stroke: white;
}

.edit-profile-btn {
  background-color: #313034;
}

.edit-profile-btn:hover {
  background-color: #313034;
}
</style>

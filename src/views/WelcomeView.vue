<template>
  <div class="about">
    <h1>Welcome to the budget calculator</h1>
    <p>You'll find and easy calculator that will help you to get an estimated cost about the services you'd like to purchase.</p>
    <b-button @click="$router.push('/pressupost')" class="main-btn">START</b-button>
  </div>
</template>
<style scoped>

.main-btn {
  background-color: #1ba8ab;
  border-color: #0a6869;
  color: white;
}
.main-btn:hover {
  background-color: #54c3c5;
}
</style>
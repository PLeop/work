<template>
  <img alt="Vue logo" src="./assets/logox.png">
  <HomeView msg="GuardianGames - Titel oder sowas"/>
  <div id ="nav">
    <a href="#">Home</a> |
    <a href="#/register">Register</a> |
    <a href="#/login">Login</a> |
    <a href="#/tower">Tower</a>
  </div>
  <component :is="currentView" />
</template>

<script setup>

//#region imports

import {ref, computed} from 'vue'
import axios from 'axios'

import RegisterView from './components/Register.vue'
import LoginView from './components/Login.vue'
import HomeView from './components/Home.vue'
import TowerView from './components/Tower.vue'

//#endregion

//#region routing and access control

const routesToCheck = ["#/login", "#/register"];
const routesToCheckForAccess = ['#/tower'];

const routes ={
  '/register': RegisterView,
  '/login' : LoginView,
  '/#': HomeView,
  '/tower' : TowerView
}

const currentPath = ref(window.location.hash)

window.addEventListener('hashchange', () => {
  currentPath.value = window.location.hash
  //Check if user is already logged in
  var token = localStorage.getItem('accessToken');
  if(routesToCheck.includes(currentPath.value)){
    if(token != null){
      axios.defaults.headers.common['Authorization'] = `Bearer ${token}`;
      axios.get('https://localhost:7013/Access')
        .then(() =>{
          //User is logged in, so redirect to tower
          window.location.href = '/#/tower';
        })
        .catch((error) =>{
          //User is not in or token is not valid, so write error if there is one, else do nothing and continue log in process
          console.log(error);
        })
    }
  }

  //Check if user has Access to tower = is logged in
  if(routesToCheckForAccess.includes(currentPath.value)){
    if(token != null){
      axios.defaults.headers.common['Authorization'] = `Bearer ${token}`;
      axios.get('https://localhost:7013/Access')
        .then(() =>{
          //token is there and user is logged in, so continue with opening the tower
        })
        .catch((error) =>{
          //invalid token, redirect to login
          window.location.href = '/#/login';
          console.log(error);
          console.log('Please log in first');
        })
    }
    else
    {
      //invalid token or no token, redirect to login
      window.location.href = '/#/login';
      console.log('Please log in first');
    }
  }



})

const currentView = computed(() => {
  return routes[currentPath.value.slice(1) || '/'] 
})

//#endregion


</script>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: -4%;
}
</style>

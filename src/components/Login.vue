<template>
    <div class="login">
      <h1>{{ msg }}</h1>
      <div id="userData">
        <input v-model="idInput" placeholder="E-Mail or Username"/>
        <input v-model="secretInput" placeholder="Secret" type="password"/>
        <button @click="Login(this.idInput, this.secretInput)">Login</button>
      </div>
    </div>
  
  </template>


<script>

import axios from 'axios'

//

export default {
    name: 'LoginView',
    props: {
        msg: String,
        //answer: String
    },
    data() {
        return {
            idInput : '',
            secretInput : ''
        }
    },
    methods: {
      async Login(username, secret){
        axios.post('https://localhost:7013/Login?id='+username+'&secret='+secret)
      .then(response=>{
        //console.log(response);  
        localStorage.setItem('accessToken', response.data);
        window.location.href = '/#/tower';
      })
      }
    }
}
</script>
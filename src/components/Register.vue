<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
      <div id="characterSelection">
        <img id="titanImg" :src="titan" @click="characterSelect('Titan')" v-if="!characterChoiceDone">
        <img id="warlockImg" :src="warlock" @click="characterSelect('Warlock')" v-if="!characterChoiceDone">
        <img id="hunterImg" :src="hunter" @click="characterSelect('Hunter')" v-if="!characterChoiceDone">
      </div>
      <div id="userData">
        <input v-model="emailInput" placeholder="E-Mail" v-if="!characterChoiceDone"/>
        <input v-model="usernameInput" placeholder="Username" v-if="!characterChoiceDone"/>
        <input v-model="secretInput" placeholder="Secret" v-if="!characterChoiceDone" type="password" />
        <p>{{ userCreation }}</p>
        <a v-if="characterChoiceDone" href="#/login" >Login now</a>
      </div>      
  </div>

</template>

<script>

import titan from "../assets/titan.jpeg"
import warlock from "../assets/warlock.png"
import hunter from "../assets/hunter.jpeg"
import axios from 'axios'

export default {
  name: 'RegisterView',
  props: {
    msg: String,
    //answer: String
  },
  data() {
    return {
      answer: {},
      res:{},
      titan: titan,
      warlock: warlock,
      hunter: hunter,
      charSelection:{},
      characterChoiceDone : false,
      usernameInput : '',
      emailInput : '',
      secretInput : '',
      userCreation : '',
    };
  },
  methods: {
    async characterSelect(character){
      //console.log(this.usernameInput, + '    ' + this.emailInput + '       ' + this.secretInput)
      axios.post('https://localhost:7013/UserCreation?username='+this.usernameInput+'&email='+this.emailInput+'&secret='+this.secretInput+'&classChoice='+character)
      .then(response=>{
        console.log(response);
        this.userCreation = response.data;
        this.characterChoiceDone = true;
      })
      .catch(AxiosError => {
        console.log(AxiosError);
        this.userCreation = 'User creation failed. ' + AxiosError.response.data;
        //error handling falls keine verbindung zum Server besteht 
      })

    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

#controls{
  margin-top:2%;
}


#userData{
  margin-top: 2%;
}

#characterSelection{
  margin-top:5%;
  /*margin-bottom: 5%;*/
  display: block;
}

#titanImg{
  float: left;
  max-width: 20%;
  max-height: 50%;
  margin-left: 15%;
  margin-right: 5%;
}

#warlockImg{
  float: left;
  max-width: 20%;
  max-height: 50%;
  margin-right:5%
}

#hunterImg{
  float: left;
  max-width: 20%;
  max-height: 50%;
  margin-right:15%;
}

h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

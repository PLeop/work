<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button @click="getPosts">Button</button>
    <p>{{ answer }}</p>
    <button @click="getPrice">GetPrice</button>
  </div>

  <!--form action="https://steamcommunity.com/openid/login" method="post">
        <input type="hidden" name="openid.identity"
               value="http://specs.openid.net/auth/2.0/identifier_select" />
        <input type="hidden" name="openid.claimed_id"
               value="http://specs.openid.net/auth/2.0/identifier_select" />
        <input type="hidden" name="openid.ns" value="http://specs.openid.net/auth/2.0" />
        <input type="hidden" name="openid.mode" value="checkid_setup" />
        <input type="hidden" name="openid.realm" value="https:\\yourOpenIdRealm.com" /-->
        <!--input type="hidden" name="openid.return_to" value="http://localhost:8080/" />
        <Button type="submit">Log in through Steam</Button>
    </form-->

</template>

<script>

import axios from 'axios'

export default {
  name: 'MainView',
  props: {
    msg: String,
    //answer: String
  },
  data() {
    return {
      answer: {},
      res:{},
    };
  },
  methods: {
    //async getAnswer() {
      //const { data } = await axios.get("https://api.sampleapis.com/bitcoin/historical_prices");
      //this.answer = data;
      
    //},
    async getPrice(){
      axios.get('https://api.steamapis.com/market/item/730/AK-47 | Redline (Field-Tested)?api_key=UyUN-NrQGqeuJcDTlCtaeqPb4IE')
        .then(response => {
            this.res = response.data;
            //console.log(this.res);
            console.log(this.getAveragePrice(this.res));

        })
    },
    async getPosts() {
      axios.get('https://api.sampleapis.com/bitcoin/historical_prices')
        .then(response => {
            this.answer = response.data;
            this.transformResponse(this.answer);

        })
        .catch(error =>{
            this.errors.push(error);
            console.log(error);
        })
    },
    transformResponse(response){
      if(response.length >= 1){
        console.log('success');
      }
    },
    getAveragePrice(someResponse){
      if(someResponse != null){
        var totalPrice = 0;
        var totalSales = 0;
        someResponse.median_avg_prices_15days.forEach(day=>{
          if(day[2]>100){
            totalSales = totalSales + day[2];
            totalPrice = totalPrice + day[1] * day[2];
            //console.log('Verkäufe: ' + day[2]);
          }
        })
        console.log(totalSales + ' Verkäufe für insagesamt ' + totalPrice + '$');
        console.log('Druchschnittspreis: ' + totalPrice/totalSales);
        return 'success';
      }
      
    }
  }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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

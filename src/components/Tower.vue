<template>
     <div id="ressources">
        <p>Legendary Shards: {{ legendaryShards }}</p>
        <p>Ascendent Shards: {{ ascendentShards }}</p>
        <p>Dusklight Shards: {{ dusklightShards }}</p>
        <p>Microphasic Datalattice: {{ microphasicDatalattice }}</p>
        <p>Helium Filaments: {{ heliumFilaments }}</p>
        <p>Baryon Boughs: {{ baryonBoughs }}</p>
        <p>Spinmetal Leaves: {{ spinmetalLeaves }}</p>
        <p>Glacial Starwort: {{ glacialStarwort }}</p>
        <!--button @click="RefreshRessources()">Refresh</button-->
    </div>
    <div class="tower">
        <img :src="tower">
    </div>
</template>

<script>

import tower from "../assets/TowerPlaza.webp"

//import VueCookies from 'vue-cookies'
import axios from 'axios'

export default {
    name: 'TowerView',
    mounted(){
    this.RefreshRessources();
    },
    data() {
        return {
            tower : tower,
            legendaryShards : this.legendaryShards,
            ascendentShards : this.ascendentShards, 
            dusklightShards : this.dusklightShards,
            microphasicDatalattice : this.microphasicDatalattice,
            heliumFilaments : this.heliumFilaments,
            baryonBoughs : this.baryonBoughs,
            spinmetalLeaves : this.spinmetalLeaves,
            glacialStarwort : this.glacialStarwort
        }
    },
    methods: {
        async RefreshRessources(){
            var token =  localStorage.getItem('accessToken');
            //username vom Backend holen --> GetUsername oder so
            axios.defaults.headers.common['Authorization'] = `Bearer ${token}`;
            axios.get('https://localhost:7013/Access/GetUserName')
            .then((response) => {
                var username = response.data;
                axios.get('https://localhost:7013/Action?username='+username)
                .then((response) =>{
                //User is logged in, so redirect to tower
                    console.log(response.data)
                    this.legendaryShards = response.data.legendaryShards;
                    this.ascendentShards = response.data.ascendentShards;
                    this.dusklightShards = response.data.dusklightShards;
                    this.microphasicDatalattice = response.data.microphasicDatalattice;
                    this.heliumFilaments = response.data.heliumFilaments;
                    this.baryonBoughs = response.data.baryonBoughs;
                    this.spinmetalLeaves = response.data.spinmetalLeaves;
                    this.glacialStarwort = response.data.glacialStarwort;
                })
            })
            .catch(() =>{
                console.log('Could not retrieve username');
            })
        }    
    }
}   

</script>
<template>
     <div id="ressources">
        <p>Legendary Shards</p>
        <p>Ascendent Shards</p>
        <p>Dusklight Shards</p>
        <p>Microphasic Datalattice</p>
        <p>Helium Filaments</p>
        <p>Baryon Boughs</p>
        <p>Spinmetal Leaves</p>
        <p>Glacial Starwort</p>
    </div>
    <div class="tower">
        <img :src="tower">
    </div>
</template>

<script>

import tower from "../assets/TowerPlaza.webp"

//import VueCookies from 'vue-cookies'
//import axios from 'axios'

export default {
    name: 'TowerView',
    data() {
        return {
            tower : tower
        }
    },
    methods: {
        
    }
}   

</script>
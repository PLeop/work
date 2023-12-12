<template>
    <div class="home">
      <h1>{{ msg }}</h1>
    </div>
  
</template>

<script>

export default {
    name: 'HomeView',
    props: {
        msg: String,
        //answer: String
    }
}
</script>
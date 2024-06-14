<template>
<div class="container">
    <img :src="imagen" alt="no encuentra la imagen">
    <p>{{texto}}</p>
</div>
  
</template>

<script>
export default {

    data() {
        return {

        }
    },

    props:{
        imagen:String,
        texto:String

    }

}
</script>

<style>

</style>
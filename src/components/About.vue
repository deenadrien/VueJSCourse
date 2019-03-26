<template lang="html">

  <section class="about">
        <img src="@/assets/question.png" id="img_header">
        <h1 class="text-center">A propos</h1>
        <p class="text-center">Application réalisée à l'aide de Bootstrap & VueJS</p>
        <div class="row">
            <div class="col-sm-6 text-right">
                <img src="@/assets/bootstrap.png" width="30%">
            </div>
            <div class="col-sm-6">
                <img src="@/assets/logo.png">
            </div>         
        </div>
  </section>

</template>

<script lang="js">

  export default  {
    name: 'about',
    props: [],
    mounted() {
    },
    data() {
      return {

      }
    },
    methods: {
    }
}
</script>

<style scoped>
    #img_header{
        width: 10%;
        display: block;
        margin-left: auto;
        margin-right: auto;
    }
</style>

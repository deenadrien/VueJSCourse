<template>
  <div id="app">
    <div id="nav" class="text-center">
      <router-link to="/">Accueil</router-link> | 
      <router-link to="/lists">Mes listes</router-link> | 
      <router-link to="/about">A propos</router-link>
    </div>
    <router-view/>
  </div>
</template>

<style>
  @import '../node_modules/bootstrap/dist/css/bootstrap.css';
</style>
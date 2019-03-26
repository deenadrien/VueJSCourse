<template lang="html">

  <section class="listOfLists">
    <img src="@/assets/delivery.png" id="img-head">
    <h1 class="text-center">Toutes mes listes</h1>
    <hr>
    <div id="add-list">
      <input class="form-control" type="text" v-model="name" placeholder="Nouvelle liste" @keyup.enter="createList()">
      <button class="btn btn-success" @click="createList()">Ajouter</button>
    </div>
    <hr>
    <input class="form-control" type="text" v-model="searchWord" placeholder="Recherche...">
    <div v-if="searchWord">
      <ul>
        <li v-for="possibility in getFilteredPossibilities" :key="possibility">{{ possibility.label }}</li>
      </ul>
    </div>
    <hr>
    <div id="list-of-lists">
      <div class="card" v-for="(item, index) in lists" :key="index">
        <div class="card-body">
          <img src="@/assets/garbage.png" height="25px" class="garbage-icon" @click="removeList(index)">
          <h5 @click="goToList(item.id, item.name)">{{item.name}}</h5>
        </div>
      </div>
    </div>
  </section>

</template>

<script lang="js">
  import router from '../router.js';

  export default  {
    name: 'listOfLists',
    props: [],
    mounted() {
      if (localStorage.getItem('shoppingLists')) {
        try {
          this.lists = JSON.parse(localStorage.getItem('shoppingLists'));
        } catch(e) {
          localStorage.removeItem('shoppingLists');
        }
      }
    },
    data() {
      return {
        searchWord: '',
        lists:[],
        name:'',
        id: 0
      }
    },
    methods: {
      createList: function(){
        if(this.name){
          if(this.lists.length > 0){
            this.id = this.lists[this.lists.length - 1].id + 1;
            this.lists.push({id: this.id, name: this.name});
          }else{
            this.id = 1;
            this.lists.push({id: this.id, name: this.name});
          }
        }
        const parsed = JSON.stringify(this.lists);
        localStorage.setItem('shoppingLists', parsed);
      },
      removeList: function(index){
        this.lists.splice(index);
         const parsed = JSON.stringify(this.lists);
        localStorage.setItem('shoppingLists', parsed);
      },
      goToList: function(id,name){
        router.push("/list/" + id + "/" + name);
      }
    },
    computed: {
      getFilteredPossibilities: function(){
        return this.lists.filter(possibility => possibility.includes(this.searchWord));
      }
    }
}
</script>

<style scoped>
  hr{
    width: 75%;
  }

  .form-control{
    width: 40%;
    display: inline;
    margin-right: 1%;
  }

  #add-list{
    width: 70%;
    display: block;
    margin-left: auto;
  }

  .card{
    margin-top: 1%;
    margin-bottom: 1%;
  }

  h5{
    display: inline;
  }

  .btn-delete{
    margin-left : 1%;
    display: inline;
  }

  .garbage-icon{
    margin-right: 2%;
  }

  #img-head{
    margin-left: auto;
    margin-right: auto;
    width: 5%;
    display: block;
  }
</style>

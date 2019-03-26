<template>
    <div>
        <div class="container">
            <h1 class="text-center">{{ $route.params.name }}</h1>
            <hr>
            
            <h3 class="align-left">Total : {{ total }} €</h3>
            <div v-if="checkMax" class="alert alert-danger" role="alert">
                <b>Attention</b>, ton budget est dépassé !
            </div>
            <div class="input-group input-group-sm mb-3">
                <div class="input-group-prepend">
                    <span class="input-group-text" id="inputGroup-sizing-sm">Budget Maximum</span>
                </div>
                <input type="text" class="form-control" v-model="budgetMax" aria-label="Sizing example input" @onkeyup="saveBudgetMax()"  aria-describedby="inputGroup-sizing-sm">
            </div>
            <hr>
            <div class="input-group mb-3">
                <div class="input-group-prepend">
                    <span class="input-group-text" id="inputGroup-sizing-default">Label</span>
                </div>
                <input type="text" class="form-control" v-model="libelle" @keydown.enter="addRowToList" aria-label="Sizing example input" aria-describedby="inputGroup-sizing-default">
            </div>
            <div v-if="libelle">
                <ul>
                    <li v-for="possibility in getFilteredPossibilities" :key="possibility" @click="addRowToListFromAutoComplete(possibility)">{{ possibility }}</li>
                </ul>
            </div>
            <button type="button" class="btn btn-info" @click="addRowToList">Ajouter</button>
            <div v-for="(item, index) in list" :key="index" >
                <div class="card w-75" v-if="item.idList == $route.params.id">
                    <div class="card-body">
                        <div class="form-group form-check form-inline-position">
                            <input type="checkbox" v-model="item.checked" class="form-check-input">
                            <label class="form-check-label" for="exampleCheck1">Acheté  </label>
                        </div>
                        <h6 class="card-inline">{{ item.label }}</h6>&nbsp;
                        <input type="text" v-model="item.price" v-if="item.checked" class="form-control card-inline">
                        <button class="btn btn-danger" @click="deleteProduct(index)">Supprimer</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    name: 'List',
    components:{
    },
    data: function(){
        return{
            title: '',
            budgetMax: 0,
            checked: false,
            libelle: '',
            list:[],
            searchWord: '',
            listOfWords: [
            "carottes",
            "riz",
            "saucisses",
            "jambon",
            "saucisson"
            ]
        }
    },
    mounted() {
        if (localStorage.getItem('shoppingList')) {
            try {
                this.list = JSON.parse(localStorage.getItem('shoppingList'));
            } catch(e) {
                localStorage.removeItem('shoppingList');
            }
        }
        if (localStorage.getItem('budgetList')) {
            try {
                this.list = JSON.parse(localStorage.getItem('budgetList'));
            } catch(e) {
                localStorage.removeItem('budgetList');
            }
        }
    },
    methods:{
        deleteProduct: function(index){
            this.list.splice(index);
            const parsed = JSON.stringify(this.list);
            localStorage.setItem('shoppingList', parsed);
        },
        addRowToList: function(){
            if(!this.libelle){
                return;
            }
            this.list.push({label: this.libelle,idList: this.$route.params.id, price: 0, checked: false});
            this.label = '';
            const parsed = JSON.stringify(this.list);
            localStorage.setItem('shoppingList', parsed);
        },
        addRowToListFromAutoComplete: function(item){
            this.list.push({label: item,idList: this.$route.params.id, price: 0, checked: false});
            this.label = '';
            const parsed = JSON.stringify(this.list);
            localStorage.setItem('shoppingList', parsed);
        },
        checkTotal: function(){
            if(this.total > this.budgetMax){
                return this.budgetCheck = true;
            }else{
                return this.budgetCheck = false;
            }
        },
        saveToLocal: function(){
            const parsed = JSON.stringify(this.list);
            localStorage.setItem('shoppingList', parsed);
        },
        saveBudgetMax: function(){
            let budgetList = {idList: this.$route.params.id, budgetMax: this.budgetMax};
            
            const parsed = JSON.stringify(budgetList);
            localStorage.setItem('budgetList', parsed);
        }
    },
    computed:{
        total: function(){
            let total = 0;
            for(let i = 0; i < this.list.length; i++){
                if(this.list[i].idList == this.$route.params.id){
                    if (this.list[i].price !== undefined){
                        total = parseInt(total) + parseInt(this.list[i].price);
                    }     
                } 
            }
            return total;
        },
        checkMax: function(){
            let budgetCheck = false;
            if(this.total > this.budgetMax){
                budgetCheck = true;
            }else{
                budgetCheck = false;
            }
            return budgetCheck;
        },
        getFilteredPossibilities: function(){
            return this.listOfWords.filter(possibility => possibility.includes(this.libelle));
        }
    }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    h6{
        margin-left: 3%;
    }

    .card{
        margin: 1%;
    }

    .card-inline{
        display: inline;
    }

    .display-inline{
        display: inline;
    }

    #title{
        display: inline;
    }

    .alert{
        margin-top: 2%;
    }

    #libelle_input{
        width: 85%;
    }

    .form-inline-position{
        display: inline;
    }

    .input-prix{
        width: 40%;
    }

    li{
        list-style-type: none;
        background-color: #576574;
        color: #c8d6e5;
        font-weight: bold;
    }

    li:hover{
        list-style-type: none;
        background-color: #c8d6e5;
        color: #222f3e;
        font-weight: bold;
    }
</style>

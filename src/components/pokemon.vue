<template>
    <div class="pokemon">

        <div class="card">
        <div class="card-image">
            <figure class="">
            <img :src="this.currentImg" alt="Placeholder image">
            </figure>
        </div>
        <div class="card-content">
            <div class="media">

            <div class="media-content">
                <p class="title is-4">{{num}} - {{name | upper}}</p>
                <p class="subtitle is-6">{{pokemon.type}}</p>
            </div>
            </div>

            <div class="content">
                <button class="button is-primary button is-fullwidth" @click="mudarSprite">Mudar Sprite</button>
                <button v-if="isShiny === false" class="button is-danger is-fullwidth botao" @click="shiny">Shiny</button>
                <button v-else class="button is-info is-fullwidth botao" @click="shiny">Shiny</button>
            </div>
        </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';

export default {
    created: function(){
        axios.get(this.url).then(res =>{
            console.log(res)
            this.pokemon.type = res.data.types[0].type.name;
            this.pokemon.front = res.data.sprites.front_default;
            this.pokemon.back = res.data.sprites.back_default;
            this.pokemon.front_shiny = res.data.sprites.front_shiny;
            this.pokemon.back_shiny = res.data.sprites.back_shiny;
            this.currentImg = this.pokemon.front;
        })
    },
    data(){
        return{
            isFront:true,
            isShiny:false,
            currentImg:'',
            pokemon: {
                type:"",
                front:"",
                back:"",
                front_shiny:"",
                back_shiny:""
            }
        }
    },
    props:{
        num:Number,
        name:String,
        url:String
    },
    filters:{
        upper: function(value){
            let newName = value[0].toUpperCase() + value.slice(1);
            return newName;
        }
    },
    methods:{
        mudarSprite: function(){
            if(this.isFront === true && this.isShiny == false){
                this.isFront = false;
                this.currentImg = this.pokemon.back;
            }else if(this.isFront === false && this.isShiny == false){
                this.isFront = true;
                this.currentImg = this.pokemon.front;
            }else if(this.isFront === true && this.isShiny == true){
                this.isFront = false;
                this.currentImg = this.pokemon.back_shiny;
            }else{
                this.isFront = true;
                this.currentImg = this.pokemon.front_shiny;
            }
        },
        shiny: function(){
            this.isShiny = !this.isShiny; 
            console.log("Shiny");
        }
    }
}
</script>
<style scoped>
.pokemon{
    margin-top:1% ;
}
.botao{
    margin-top:1% ;
}
</style>
<template>
    <div class="mt-4">
        <div class="card">
            <div class="card-image">
                <figure>
                <img :src="currentImg" @mouseover="mudarSprite" @mouseleave="mudarSprite" alt="Placeholder image">
                <img :src="currentShiny" @mouseover="mudarSpriteShiny" @mouseleave="mudarSpriteShiny" alt="Placeholder image">
                </figure>
            </div>

            <div class="card-content">
                <div class="media">
                <div class="media-content">
                    <p class="title is-4">{{num}} - {{name | upper}}</p>
                    <p class="subtitle is-6">{{pokemon.type1}} {{pokemon.type2}}</p>
                </div>
                </div>

                <div class="content">
                    <button class="button is-fullwidth" @click="mudarSprite">Mudar sprite</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    created: function(){
        axios.get(this.url).then(res => {
            
            this.pokemon.type1 = res.data.types[0].type.name;
            if(res.data.types.length > 1){
                this.pokemon.type2 = res.data.types[1].type.name;
            }
            this.pokemon.front = res.data.sprites.front_default;
            this.pokemon.back = res.data.sprites.back_default;
            this.pokemon.frontShiny = res.data.sprites.front_shiny;
            this.pokemon.backShiny = res.data.sprites.back_shiny;
            this.currentImg = this.pokemon.front;
            this.currentShiny = this.pokemon.frontShiny;
            // console.log(this.pokemon);
        })
    },
    data(){
        return{
            isFront: true,
            hover: false,
            currentImg: '',
            currentShiny: '',
            pokemon: {
                type1: '',
                type2: '',
                front: '',
                back: '',
                frontShiny: '',
                backShiny: ''
            }
        }
    },
    props: {
        num: Number,
        name: String,
        url: String
    },
    filters: {
        upper: function(value) { //recebendo o texto, separando em duas partes, para colocar a primeira letra como maiuscula.
            var newName = value[0].toUpperCase() + value.slice(1);
            return newName;
        }
    },
    methods: {
        mudarSprite: function() {
            if(this.isFront){
                this.isFront = false;
                this.currentImg = this.pokemon.back;
            } else {
                this.isFront = true;
                this.currentImg = this.pokemon.front;
            }
        },
        mudarSpriteShiny: function(){
            if(this.isFront){
                this.isFront = false;
                this.currentShiny = this.pokemon.backShiny;
            } else {
                this.isFront = true;
                this.currentShiny = this.pokemon.frontShiny;
            }
        }
    }
}
</script>

<style>

</style>

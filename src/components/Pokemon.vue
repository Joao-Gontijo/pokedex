<template>
    <div class="mt-4">
        <div class="card">
            <div class="card-image">
                <figure>
                <img :src="currentImg" alt="Placeholder image">
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
            this.currentImg = this.pokemon.front;
            // console.log(this.pokemon);
        })
    },
    data(){
        return{
            isFront: true,
            currentImg: '',
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
            
        }
    }
}
</script>

<style>

</style>

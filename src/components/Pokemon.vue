<template>
    <div class="mt-4 ">
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
                    <p class="title is-4">{{pokemon.dexNumber}} - {{name | upper}}</p>
                    <p class="subtitle is-6">{{pokemon.type1}} {{pokemon.type2}}</p>
                </div>
                </div>

                <div class="content">
                    <button class="button is-fullwidth" @click="showEntry = true">Entries</button>
                </div>
                <div class="modal" v-bind:class="{'is-active': showEntry}">
                    <div class="modal-background" v-on:click="showEntry = false"></div>
                    <div class="modal-card">
                        <header class="modal-card-head">
                        <p class="modal-card-title">{{pokemon.nome}}</p>
                        <button class="delete" aria-label="close" v-on:click="showEntry = false"></button>
                        </header>
                        <section class="modal-card-body">
                            {{pokemon.description}}
                        </section>
                        <footer class="modal-card-foot">
                        <button class="button" v-on:click="showEntry = false">Cancel</button>
                        </footer>
                    </div>
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

            this.pokemon.nome = res.data.forms[0].name;
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
            this.pokemon.dexNumber = res.data.id;

            axios.get(res.data.species.url).then(res => {
                for (let index = 0; index < res.data.flavor_text_entries.length; index++) {
                    if(res.data.flavor_text_entries[index].language.name == 'en'){
                        this.pokemon.description = res.data.flavor_text_entries[index].flavor_text
                    }
                }
            })
        })
    },
    data(){
        return{
            showEntry: false,
            isFront: true,
            hover: false,
            currentImg: '',
            currentShiny: '',
            pokemon: {
                nome: '',
                type1: '',
                type2: '',
                front: '',
                back: '',
                frontShiny: '',
                backShiny: '',
                description: '',
                dexNumber: Number
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
        },
        // mostrarEntry: function(){
        //     console.log(this.pokemon.description);

        // }
    }
}
</script>

<style>
.column-wrapper{
    column-count: 4;
}
 *:first-letter{
     text-transform: capitalize;
 }
</style>
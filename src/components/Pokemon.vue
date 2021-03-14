<template>
    <div id='pokemon'>
        <div class="card">
            <div class="card-image">
                <figure class="">
                    <img :src=currentImg alt="Placeholder image">
                </figure>
            </div>
            <div class="card-content">
                <div class="media">
                    <div class="media-content">
                        <p class="title is-4">{{nome | upper}}</p>
                        <p class="subtitle is-6">{{pokemon.type.name}}</p>
                    </div>
                </div>
            </div>
            <div class="content">
                <button style="margin: 2%" class="button is-info is-rounded" @click=virarPokemon>Virar Pokemon</button>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    props:{
        numero:Number,
        nome:String,
        url:String,
    },
    filters: {
        upper: function(value){
            let nomeUpper = value[0].toUpperCase() + value.slice(1);
            return nomeUpper;
        }
    },
    data(){
        return{
            isFront: true,
            currentImg : '',
            pokemon: {
                type:'',
                front:'',
                back:''
            }
        }
    },
    created: function(){
        axios.get(this.url).then(res=>{
            this.pokemon.type = res.data.types[0].type;
            this.pokemon.front = res.data.sprites.front_default;
            this.currentImg = this.pokemon.front;
            this.pokemon.back = res.data.sprites.back_default;
            console.log(this.pokemon.front)

        })
    },
    methods:{
        virarPokemon: function(){
            if(this.isFront){
                this.isFront = false;
                this.currentImg = this.pokemon.back;
            }else{
                this.isFront = true;
                this.currentImg = this.pokemon.front;
            }
        }
    }
}
</script>

<style>
#pokemon{
    margin: 1%
}
</style>
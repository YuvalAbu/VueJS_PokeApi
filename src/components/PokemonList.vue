<template>
	<div id="toto">
		<h1 style="text-align:center">List Pokemon</h1>
		<v-card
    max-width="500"
    class="mx-auto"
  >
    <v-toolbar
      color="indigo"
      dark
    >
		<v-toolbar-title>Pokemons</v-toolbar-title>
		</v-toolbar>

			<v-list>
				<v-list-item
					v-for="pokemon in orderedPokemons" :key="pokemon.id"
				>

					<v-list-item-content>
						<v-list-item-title style="font-weight: bold;">Id:{{pokemon.id}}</v-list-item-title>
						<v-list-item-title style="font-style: italic;">Name: {{pokemon.name | capitalize}}</v-list-item-title>
						<v-list-item-title style="font-style: italic;"> Type:
							<v-list-item-subtitle style="font-weight: bold;"  v-for="type in pokemon.types" :key="type.solt">{{type.type.name}}</v-list-item-subtitle>
						</v-list-item-title>
					</v-list-item-content>
		
					<v-list-item-avatar>
						<v-img :src="imageUrl + pokemon.id + '.png'"></v-img>
					</v-list-item-avatar>
				</v-list-item>
			</v-list>
			</v-card>
			<div id="scroll-trigger" ref="infinitescrolltrigger"><i class="fas fa-spinner fa-spin"></i></div>	
	</div>
</template>
<script>
import axios from 'axios';
import _ from 'lodash';
export default {
	props: [
		'imageUrl',
		'apiUrl'
	],
	data(){
		return {
			pokemons: [],
			urlPokemon:[],
			newUrl: '',
			nextUrl: ''
		}
	},
	methods: {
		fetchdata(newUrl) {
			axios.get(newUrl)
			.then(response =>{
				this.urlPokemon = response.data.results
				this.nextUrl = response.data.next
				response.data.results.forEach(urlPokemon => {
					axios.get(urlPokemon.url)
					.then(res =>{
						this.pokemons.push(res.data)
					})
					.catch(error => {
						/* eslint-disable no-console */
							console.error(error)
						/* eslint-enable no-console */ 
					})
				});
			})
			.catch(error => {
				/* eslint-disable no-console */
				console.log(error)
				/* eslint-enable no-console */ 
			})
		},
		scroll(){
			window.onscroll = () => {
				if (window.scrollY >= (document.documentElement.scrollHeight - window.innerHeight - 3)) {
					this.fetchdata(this.nextUrl)
				}
			}
		}
	},
  mounted() {
			this.fetchdata(this.newUrl)
			if (this.nextUrl != null) {
				this.scroll()
			}
	},
	created() {
		this.newUrl = this.apiUrl;
	},
	filters: {
		capitalize: function (value) {
			if (!value) return ''
			value = value.toString()
			return value.charAt(0).toUpperCase() + value.slice(1)
			}
	},
	computed: {
		orderedPokemons: function(){
			return _.orderBy(this.pokemons, 'id')
		}
	}
}
</script>


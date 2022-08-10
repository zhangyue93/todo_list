<template>
		<v-footer padless>
		    <v-col
		      class="text-center"
		      cols="12"
		    >
		      {{copyright}}
		    </v-col>
		  </v-footer>
</template>

<script>
	export default{
		name:"t-foot",
		data(){
			return{
				copyright:"Vuetify demo 2020-08-09"
			}
		}
	}
</script>

<style>
</style>
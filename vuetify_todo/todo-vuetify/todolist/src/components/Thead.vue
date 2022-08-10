<template>
		<v-banner 
		padless
		color="#006064"
		single-line
		rounded
		>
			<v-col
			class="text-center font-weight-black text-h3 white--text"
			cols="12"
			>{{title}}
			</v-col>
		</v-banner>

</template>

<script>
	export default{
		name:"t-head",
		data(){
			return{
				title:"ToDoリスト"
			}
		}
	}
</script>

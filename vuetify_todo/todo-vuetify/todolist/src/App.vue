<template>
<v-app>
  <div id="app">
	<t-header></t-header>
	<t-body class="space"></t-body>
	<t-foot></t-foot>
  </div>
</v-app>
</template>

<script>
import Tbody from './components/Tbody.vue';
import Thead from './components/Thead.vue';
import Tfoot from './components/Tfoot.vue';

export default {
	name: 'App',
	components: {
		"t-header":Thead,
		"t-body":Tbody,
		"t-foot":Tfoot
  },

  data: () => ({
    
  }),
};
</script>

<style>
	.space{
		margin-top: 80px;
		margin-bottom: 80px;
		margin-left: 30px;
	}
</style>
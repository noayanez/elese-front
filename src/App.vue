<template>
	<div class="appx">
		<app-header></app-header>
		<router-view/>
		<app-footer style="bottom: 0px;"></app-footer>
	</div>
</template>

<script>
	import Header from './components/Header.vue';
	import Footer from './components/Footer.vue';
	export default {
		components: {
			'app-header' : Header,
			'app-footer' : Footer
		}
	}
</script>
 
<style lang="scss">
	.appx{
		font-family: 'Roboto', sans-serif;
		background: linear-gradient(to bottom right, #00BCD4, #3F51B5);
		min-height: 100vh;
	}
</style>
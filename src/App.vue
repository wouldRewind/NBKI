<template>
	<div class="app">
		<header class="app__header">
			<h3 class="app__title">
				<a href="https://www.nbki.ru/">НБКИ</a>
			</h3>
		</header>
		<Table/>
	</div>
</template>

<script>
import Table from './components/Table/Index.vue'
export default {
	name: 'App',
	components: {
		Table,
	}
}
</script>

<style lang="scss">


body {
	@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap');
}

*, *::after, *::before {
	margin: 0;
	padding: 0;
	list-style-type: none;
	text-decoration: none;
	outline: none;
}

#app {
	font-family: 'Roboto', sans-serif;
}
</style>

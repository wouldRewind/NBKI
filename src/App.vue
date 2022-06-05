<template>
	<div class="app">
		<header class="app__header">
			<h3 class="app__title">
				<a target="_blank" href="https://www.nbki.ru/">НБКИ</a>
			</h3>
			<Button
			@generate="handleClick"
			/>
		</header>
		<Table
		:table="table"/>
		<div class="app__total">
			Общая стоимость: <b>{{ total }}</b>
		</div>
	</div>
</template>

<script>
// Config
import { tableConfig } from './assets/config'


// Components
import Table from './components/Table.vue'
import Button from './components/Button.vue'


export default {
	name: 'App',
	components: {
    Table,
    Button,
	},
	data: () => ({
		table: [],
		totalCost: null,
	}),
	methods: {
		handleClick() {
			this.generateNewTable()
			.then(table => this.calculateTotalCost(table))
			.catch(err => alert(err))
		},
		generateNewTable() {
			return new Promise((resolve, reject) => {
				if (!Array.isArray(this.table)) {
					reject(new Error('Table should be an array!'))
				}
				this.table = []
				for (let i = 0; i < tableConfig.rows; i++) {
					this.table.push(
						this.generateRow()
					)
				}
				resolve(this.table)
			})
		},
		generateRow() {
			const { nameLength, maxPrice, minPrice, maxAmount, minAmount, alphabet } = tableConfig;
			
			const name = this.generateName(alphabet, nameLength);
			const price = this.generatePrice(maxPrice, minPrice)
			const amount = this.generateAmount(maxAmount, minAmount);
			const total = this.calculateCost(price, amount);

			return {
				name,
				price,
				amount,
				total,
			}
		},
		generateName(alphabet, length) {

			let name = '';

			for (let i = 0; i < length; i++) {
				const randomSymbol = alphabet[Math.floor(Math.random() * alphabet.length)];
				const randomSymbolInCase = Math.random() > 0.5 ? randomSymbol : randomSymbol.toUpperCase()  ; 
				name += randomSymbolInCase;
			}

			return name;
		},
		generatePrice(max, min) {
			const price = (min + (Math.random() * max)).toFixed(2);
			return price < max ? price : this.generatePrice(max, min);
		},
		generateAmount(max, min) {
			const amount = min + Math.floor(Math.random() * max);
			return amount < max ? amount : this.generateAmount(max, min) ;
		},
		calculateCost(price, amount) {
			return (price * amount).toFixed(2);
		},
		calculateTotalCost(table = []) {
			this.totalCost = table
			.reduce((acc, { total }) => acc + +total, 0)
			.toFixed(2)
		}
	},
	computed: {
		tableExists() {
			return !!this.table.length
		},
		total() {
			return this.table
			.reduce((acc, { total }) => acc + +total , 0)
			.toFixed(2) + ' руб.'
		},
	},
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
	font-size: 100%;
}
.app {
	width: 90%;
	margin: 0 auto;
	padding-top: 30px;
	&__header {
		display: flex;
		align-items: center;
		justify-content: space-between;
		margin-bottom: 15px;
	}
	&__total {
		padding: 15px 0 15px 0;
	}
}
</style>

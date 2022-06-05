<template>
	<div class="app">
		<header class="app__header">
			<h3 class="app__title">
				<a class="app__company-link" target="_blank" href="https://www.nbki.ru/">
					НБКИ
				</a>
			</h3>
			<Button
			@generate="handleClick"
			/>
		</header>
		<div
		v-if="!fakeWaiting"
		class="app__table">
			<Table
			:table="table"/>
			<div class="app__total">
				Общая стоимость : <b class="app__price">{{ totalCost }}</b>
			</div>
		</div>
		<div
		v-else 
		class="app__loader">
			<img src="./assets/images/load_dog.gif" alt=""/>
		</div>
	</div>
</template>


<script>
// Config
import { tableConfig } from './assets/config'

const fakeDelay = 1000;

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
		fakeWaiting: false,
	}),
	methods: {
		handleClick() {
			if(!this.fakeWaiting) {
				this.generateNewTable()
				.then(() => this.fakeWaiting = false)
				.catch(err => alert(err));
			}
		},
		generateNewTable() {
			return new Promise((resolve, reject) => {
				if (!Array.isArray(this.table)) {
					reject(new Error('Table should be an array!'));
					return;
				}
				this.fakeWaiting = true;
				setTimeout(() => {
					this.table = []
					for (let i = 0; i < tableConfig.rows; i++) {
						this.table.push(
							this.generateRow()
						)
					}
					resolve(this.table)
				} , fakeDelay);
			});
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
			};
		},
		generateName(alphabet, length) {
			let name = '';
			for (let i = 0; i < length; i++) {
				const randomSymbol = alphabet[Math.floor(Math.random() * alphabet.length)];
				name += randomSymbol;
			}
			return name;
		},
		generatePrice(max, min) {
			const price = (min + (Math.random() * max)).toFixed(2);
			return price < max ? price : this.generatePrice(max, min);
		},
		generateAmount(max, min) {
			const amount = min + Math.floor(Math.random() * max);
			return amount;
		},
		calculateCost(price, amount) {
			return (price * amount).toFixed(2);
		},
	},
	computed: {
		totalCost() {
			const [whole, fraction] =  this.table
			.reduce((acc, { total }) => acc + +total , 0)
			.toFixed(2)
			.split(".");
			const formattedWhole = Number(whole).toLocaleString();
			return formattedWhole + '.' + fraction + ' руб.';
		},
	},
}
</script>


<style lang="scss">
@import '@/scss/_const.scss';
body {
	@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap');
	overflow-y: scroll;
}

#app {
	font-family: 'Roboto', sans-serif;
}

*, *::after, *::before {
	margin: 0;
	padding: 0;
	list-style-type: none;
	text-decoration: none;
	outline: none;
}

.app__company-link {
	color: $blueColor;
	transition: $transition;
	&:hover {
		color: darken($color: $blueColor, $amount: 10);
	}
}
.app__loader {
	text-align: center;
}
.app {
	font-size: inherit;
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
		font-weight: 600;
		color: $blueColor;
	}
	&__price {
		text-decoration: underline;
	}
}

@media (max-width: 720px) {
	.app {
		width: 100%;
		&__header {
		padding: 0 15px;
		}
		&__total {
			padding-left: 15px;
		}
	}
}
</style>

<template>
	<table class="table">
		<tr class="table__row table__header">
			<th class="table__data">Наименование</th>
			<th class="table__data">Цена</th>
			<th class="table__data">Количество</th>
			<th class="table__data">Стоимость</th>
		</tr>
		<tr class="table__row">
			<td class="table__data">Random</td>
			<td class="table__data">323.42</td>
			<td class="table__data">19</td>
			<td class="table__data">{{ 323.42 * 19 }}</td>
		</tr>
		<tr class="table__row">
			<td class="table__data">Random</td>
			<td class="table__data">323.42</td>
			<td class="table__data">19</td>
			<td class="table__data">{{ 323.42 * 19 }}</td>
		</tr>
		<tr class="table__row">
			<td class="table__data">Random</td>
			<td class="table__data">323.42</td>
			<td class="table__data">19</td>
			<td class="table__data">{{ 323.42 * 19 }}</td>
		</tr>
	</table>
</template>

<script>
export default {
	data: () => ({
		rowAmount: 50,
		table: [],
		nameLength: 5,
		maxPrice: 1000.00,
		minPrice: 0.01,
		maxAmount: 100,
		minAmount: 1,
		alphabet: ['а', 'б', 'в', 'г', 'д', 'е', 'ё', 'ж', 'з', 'и', 'й', 'к', 'л', 'м', 'н', 'о', 'п', 'р', 'с', 'т', 'у', 'ф', 'х', 'ц', 'ч', 'ш', 'щ', 'э', 'ю', 'я'],
	}),
	created() {
		this.generateNewTable().then((data) => console.log(data))
	},
	methods: {
		generateNewTable() {
			this.table = [];
			return new Promise(resolve => {
				for (let i = 0; i < this.rowAmount; i++) {
					this.table.push(
						this.generateRow()
					)
				}
				resolve(this.table)
			})
		},
		generateRow() {
			const name = this.generateName(this.alphabet, this.nameLength);
			const price = this.generatePrice(this.maxPrice, this.minPrice)
			const amount = this.generateAmount(this.maxAmount, this.minAmount);
			const total = this.calculateCost(price, amount);

			return [name, price, amount, total];
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

	},
}
</script>


<style scoped>
.table {
	border-collapse: collapse;
	width: 90%;
	margin: 0 auto;
}

.table__data {
	border: 1px solid #dddddd;
	text-align: left;
	padding: 8px;
}

</style>
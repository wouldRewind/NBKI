<template>
	<table class="table">
		<tr class="table__row table__header">
			<th
			class="table__data"
			v-for="(name, index) in header"
			:key="index">
			{{ name }}
			</th>
		</tr>
		<tr
		class="table__row"
		v-for="(row, rowIndex) in table"
		:key="rowIndex">
			<td
			class="table__data"
			v-for="(value, index) in rowValues(row)"
			:key="index">
				{{ value }}
			</td>
		</tr>
	</table>
</template>

<script>
import { tableConfig } from '@/assets/config'
const { header} = tableConfig

export default {
	name: "Table",
	props: {
		table: {
			type: Array,
			required: true,
		},
	},
	data: () => ({
		header,
	}),
	methods: {
		rowValues(row) {
			return Object.values(row)
		}
	},
}
</script>


<style lang="scss" scoped>
@import '@/scss/_const.scss';
.table {
	font-size: .875rem;
	border-collapse: collapse;
	width: 100%;
	margin: 0 auto;
}

.table__data {
	border: 1px solid $blueColor;
	text-align: left;
	padding: 8px;
}
.table__row:nth-child(odd) {
	background-color: $blueColor;
	color: $lightColor;
}
.table__row:nth-child(even) {
	background-color: $lightColor;
	color: $blueColor;
}


</style>
<template>
	<table class="table">
		<tr class="table__row table__header">
			<th
			class="table__data table__title"
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
	text-align: center;
	&.table__title {
		font-size: .95rem;
		letter-spacing: 2px;
		font-weight: 600;
		text-decoration: underline;
	}
}
.table__row {
	&:nth-child(odd) {
		background-color: $blueColor;
		color: $lightColor;	
	}
	&:nth-child(even) {
		background-color: $lightColor;
		color: $blueColor;
	}
}
@media (max-width: 720px) {
		.table {
			font-size: .7rem;
		}
		.table__title.table__data {
			font-size: .8rem;
		}
	}


</style>
<template>
	<button @click="$emit('generate')" class="generate-btn">
		Сгенерировать
	</button>	
</template>


<style lang="scss" scoped>
@import '@/scss/_const.scss';
.generate-btn {
	padding: 8px 16px;
	border: 1px solid $blueColor;
	background: $lightColor;
	border-radius: 100px;
	cursor: pointer;
	color: $blueColor;
	font-family: 'Roboto', sans-serif;
	font-weight: 600;
	font-style: .875rem;
	transition: $transition;
	&:hover {
		background: $blueColor;
		color: $lightColor;
	}
}
</style>
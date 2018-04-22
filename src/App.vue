<template>
	<div id="changeCalculatorApp">
		<div class="changeCalculatorApp--backdrop">
		</div>
		<div class="changeCalculatorApp--inner">
			<ChangeCalculator v-bind:orderTotal="orderTotal"/>
		</div>
	</div>
</template>

<script>
	import ChangeCalculator from './components/ChangeCalculator.vue'

	export default {
		name: 'changeCalculatorApp',
		components: {
			ChangeCalculator
		},
		data: function () {
			return {
				orderTotal: 199,
			};
		}
	}
</script>

<style>
	.changeCalculatorApp--backdrop {
		z-index: 1040;
		opacity: .5;
		position: fixed;
		background-color: #000;
		top: 0;
		right: 0;
		bottom: 0;
		left: 0;
	}
	.changeCalculatorApp--inner {
		font-family: 'Avenir', Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		color: #2c3e50;
		position: fixed;
		z-index: 1050;
		display: block;
		top: 0;
		right: 0;
		bottom: 0;
		left: 0;
	}
</style>

<template>
	<div class="changeCalculator">
		<div class="changeCalculator__header">
			<h3 class="changeCalculator__title">Change Calculator</h3>
		</div>
		<div style="clear:both">
			<div class="changeCalculator__totals">
				<div class="changeCalculator__total">
					<span>Order Total:</span>
					{{orderTotal | currency}}
				</div>
				<div class="changeCalculator__total">
					<span>Money Given:</span>
					{{givenMoney | currency}}
				</div>
				<div class="changeCalculator__total changeCalculator__total--bold"
					v-bind:class="{'changeCalculator__total--negative' : change < 0}">
					<span>Change:</span>
					{{change | currency}}
				</div>
			</div>
			<div class="changeCalculator__buttons">
				<input class="buttons--figure" type="button" v-on:click="enteredFigure" value="1">
				<input class="buttons--figure" type="button" v-on:click="enteredFigure" value="2">
				<input class="buttons--figure" type="button" v-on:click="enteredFigure" value="3">
				<input class="buttons--figure" type="button" v-on:click="enteredFigure" value="4">
				<input class="buttons--figure" type="button" v-on:click="enteredFigure" value="5">
				<input class="buttons--figure" type="button" v-on:click="enteredFigure" value="6">
				<input class="buttons--figure" type="button" v-on:click="enteredFigure" value="7">
				<input class="buttons--figure" type="button" v-on:click="enteredFigure" value="8">
				<input class="buttons--figure" type="button" v-on:click="enteredFigure" value="9">
				<input class="buttons--figure buttons--figure0" type="button" v-on:click="enteredFigure" value="0">
				<input class="buttons--removeLastFigure" type="button" v-on:click="removeLastFigure" value="←">
				<input class="buttons--completedChange" type="button" v-on:click="completedChange" value="Open Drawer">
			</div>
		</div>
		<div style="clear:both"></div>
	</div>
</template>
<style scoped>
	.changeCalculator {
		position: relative;
		width: auto;
		margin: 10px;
		background-color: #fff;
		background-clip: padding-box;
		border: 1px solid #999;
		border-radius: 6px;
		outline: 0;
		-webkit-box-shadow: 0 3px 9px rgba(0, 0, 0, .5);
		box-shadow: 0 3px 9px rgba(0, 0, 0, .5);
	}

	@media (min-width: 768px) {
		.changeCalculator {
			width: 600px;
			margin: 30px auto;
			clear: both;
		}

		.changeCalculator__totals {
			float: left;
			width: 200px;
		}

		.changeCalculator__buttons {
			float: left;
			width: 380px;
		}
	}

	.changeCalculator__header {
		min-height: 16.43px;
		padding: 15px;
		border-bottom: 1px solid #e5e5e5;
	}

	.changeCalculator__title {
		margin: 0;
		padding: 0;
		font-size: 24px;
	}

	.changeCalculator__total {
		font-weight: bold;
		text-align: right;
		padding: 10px;
		font-size: 130%;
	}

	.changeCalculator__total span {
		float: left !important;
		font-size: 75%;
		font-weight: normal;
	}

	.changeCalculator__total--bold {
		font-size: 150%;
		text-decoration: underline;
	}

	.changeCalculator__total--negative {
		color: darkred;
	}

	.changeCalculator__buttons {
		padding: 10px;
		display: grid;
		grid-template-columns: 33.33%;
		grid-gap: 10px;
		color: #444;
	}

	.buttons--figure, .buttons--removeLastFigure, .buttons--completedChange {
		background-color: #233245;
		border: 3px solid #35475e;
		color: #fff;
		border-radius: 5px;
		padding: 20px;
		font-size: 150%;
	}

	.buttons--figure0 {
		grid-column: 2 / 4;
		grid-row: 4;
	}

	.buttons--completedChange {
		grid-column: 1 / 4;
		grid-row: 5;
		background-color: #4cae4c;
		border-color: #4cae4c;
	}

</style>
<script>
	export default {
		name: 'ChangeCalculator',
		props: {
			orderTotal: {
				type: Number,
				required: true
			},
		},
		data: function () {
			return {
				// Remember if we are showing the fiver change or entered
				resetFromFiver: false,
				// Lets always start with the change of the nearest fiver upwards.
				givenMoney: Math.ceil(this.orderTotal / 500) * 500,
			};
		},
		computed: {
			change: function () {
				return parseInt(this.givenMoney) - parseInt(this.orderTotal);
			},
		},
		methods: {
			enteredFigure: function (event) {
				if (!this.resetFromFiver) {
					this.resetFromFiver = true;
					this.givenMoney = 0;
				}
				let figure = parseInt(event.srcElement.value);
				this.givenMoney = (this.givenMoney * 10) + figure;
			},
			removeLastFigure: function () {
				this.givenMoney = (this.givenMoney - (this.givenMoney % 10)) / 10;
			},
			completedChange: function () {
				// todo send event to close popup
				alert(1);
			},
		},
		filters: {
			currency: function (value) {
				// Err, yes this is horrible see
				// https://stackoverflow.com/questions/149055/how-can-i-format-numbers-as-dollars-currency-string-in-javascript
				return '£' + (value / 100).toFixed(2).replace(/./g, function (c, i, a) {
					return i && c !== "." && ((a.length - i) % 3 === 0) ? ',' + c : c;
				});
			}
		}
	}
</script>

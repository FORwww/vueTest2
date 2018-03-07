<template>
	<div class="col-sm-6 col-md-4">
		<div class="card  text-white bg-light">
			<div class="card-header">{{stock.name}}
				<small>(Price: {{stock.price}})</small>
			</div>
			<div class="card-body">
				<div class="row">
				<div class="form-group col-md-6">
					<div class="float-left">

						<input
								type="number"
								class="form-control"
								placeholder="Quanitity"
								v-model="quantity"
						>

					</div>
				</div>
				<div class="form-group col-md-6">
					<div class="float-right">
						<button class="btn btn-success"
						        @click="buyStock"
						        :disabled="insufficientFunds || quantity <= 0 "
						>{{insufficientFunds ? 'insufficientFunds' : 'Buy'}}
						</button>
					</div>
				</div>
				</div>

			</div>
		</div>
	</div>
</template>

<script>
	export default {
		props: ['stock'],
		data() {
			return {
				quantity: 0
			}
		},
		computed: {
			funds() {
				return this.$store.getters.funds;
			},
			insufficientFunds() {
				return this.quantity * this.stock.price > this.funds;
			}
		},
		methods: {
			buyStock() {
				const order = {
					stockId: this.stock.id,
					stockPrice: this.stock.price,
					quantity: this.quantity
				};
				console.log(order);
				this.$store.dispatch('buyStock', order);
				this.quantity = 0;
			}
		}

	}
</script>

<style scoped>
	.card {
		margin: 10px 0;
	}

	.card-header {
		color: #121212;
	}
</style>
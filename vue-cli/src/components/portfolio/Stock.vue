<template>
	<div class="col-sm-6 col-md-4">
		<div class="card border-info text-white bg-light">
			<div class="card-header">{{stock.name}}
				<small>(Price: {{stock.price}} | Quantity: {{stock.quantity}})</small>
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
					<button class="btn btn-info"
					        @click="sellStock"
					        :disabled="insufficientQuantity || quantity <= 0 "
					>{{ insufficientQuantity ? 'Not enough': 'Sell'}}</button>
						</div>
					</div>
				</div>

			</div>
		</div>
	</div>
</template>

<script>
	import {mapActions} from 'vuex';
	export default {
		props: ['stock'],
		data() {
			return {
				quantity: 0
			}
		},
		computed: {
			insufficientQuantity(){
				return this.quantity > this.stock.quantity;
			}
		},
		methods:{
			...mapActions({
				placeSellOrder: 'sellStock'
			}),
			sellStock(){
				const order = {
					stockId: this.stock.id,
					stockPrice: this.stock.price,
					quantity: this.quantity
				};
				this.placeSellOrder(order);
				this.quantity =  0;
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
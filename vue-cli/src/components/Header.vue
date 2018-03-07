<template>
	<div>
		<nav class="navbar navbar-expand-lg navbar-light bg-light">
			<router-link to="/" class="navbar-brand">Trader</router-link>
			<button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent"
			        aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
				<span class="navbar-toggler-icon"></span>
			</button>

			<div class="collapse navbar-collapse" id="navbarSupportedContent">
				<ul class="navbar-nav mr-auto">
					<router-link tag="li" to="/portfolio" class="nav-item" activeClass="active">
						<a class="nav-link">Portfolio</a>
					</router-link>
					<router-link tag="li" to="/stocks" class="nav-item" active-class="active">
						<a class="nav-link">Stocks</a>
					</router-link>
					<router-link tag="li" to="/gallery" class="nav-item" active-class="active">
						<a class="nav-link">Gallery</a>
					</router-link>

					<li class="nav-item dropdown" :class="{show: isDropdownOpen}">
						<a class="nav-link dropdown-toggle"
						   href="#"
						   id="navbarDropdown"
						   role="button"
						   data-toggle="dropdown"
						   aria-haspopup="true"
						   aria-expanded="false">
							Save & Load
						</a>
						<div class="dropdown-menu"
						     :class="{show: isDropdownOpen}"
						     @click="isDropdownOpen = !isDropdownOpen"
						     aria-labelledby="navbarDropdown">
							<a class="dropdown-item" href="#" @click="saveData">Save Data</a>
							<a class="dropdown-item" href="#" @click="loadData">Load Data</a>
						</div>
					</li>

					<li class="nav-item">
						<a class="nav-link" href="#" @click="endDay">End Day</a>
					</li>




				</ul>
				<div class="my-2 my-lg-0">
				<strong>Funds: {{funds | currency}}</strong>
				</div>
			</div>
		</nav>
	</div>
</template>

<script>
	import {mapActions} from 'vuex'
	export default {
		data(){
			return{
				isDropdownOpen: false
			}
		},
		computed:{
			funds(){
				return this.$store.getters.funds;
			}
		},
		methods:{
			...mapActions({
				randomizeStocks: 'randomizeStocks',
				fetchData: 'loadData'
			}),
			endDay(){
				this.randomizeStocks();
			},
			saveData(){
				const data = {
					funds: this.$store.getters.funds,
					stocks: this.$store.getters.stocks,
					stockPortfolio: this.$store.getters.stockPortfolio
				};
					this.$http.put('data.json', data);
			},
			loadData(){
				this.fetchData();
			}
		}
	}
</script>

<style scoped>
.navbar-nav .active{
	background: #28a745;
	border-radius: .25rem
}
.navbar-nav .active a{
	color: #fff!important;
}
</style>
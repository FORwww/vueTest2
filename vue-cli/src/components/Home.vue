<template>
	<div>
		<h1>Home Component</h1>
		<p>Your Funds: {{funds | currency}}</p>
	</div>


</template>

<script>
	export default{
		computed:{
			funds(){
				return this.$store.getters.funds
			}
		}
	}
</script>

<style scoped>

</style>
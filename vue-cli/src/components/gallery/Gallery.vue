<template>
	<div>
		<pagination
				:current="currentPage"
				:total="totalPhotos"
				:per-page="perPage"
				@page-changed="fetchPhotos"
		></pagination>
		<section class="grid">
			<div class="grid__item card" v-for="photo in photos">
				<div class="card__body">
					<img :src="photo.urls.small" alt="">
				</div>
				<div class="card__footer media">
					<img :src="photo.user.profile_image.small" alt="" class="media__obj">
					<div class="media__body">
						<a :href="photo.user.portfolio_url" target="_blank">{{ photo.user.name }}</a>
					</div>
				</div>
			</div>
		</section>
		<pagination
				:current="currentPage"
				:total="totalPhotos"
				:per-page="perPage"
				@page-changed="fetchPhotos"
		></pagination>

	</div>
</template>

<script>
	import Pagination from './Gallery-Pagination.vue';

	export default {
		data() {
			return {
				api: 'c0e48d302668a3e0f4ed3c9e460ee30906023bc52d76faafc7722d1779dbcaa9',
				photos: [],
				totalPhotos: 0,
				perPage: 10,
				currentPage: 1
			}
		},
		methods: {
			fetchPhotos: function (page) {
				var options = {
					params: {
						client_id: this.api,
						page: page,
						per_page: this.perPage
					}
				};

				this.$http.get('https://api.unsplash.com/photos', options).then(function (response) {
					this.photos = response.data;
					this.totalPhotos = parseInt(response.headers.get('x-total'));
					this.currentPage = page;
				}, console.log)
			}
		},
		created: function () {
			this.fetchPhotos(this.currentPage)
		},
		components: {
			pagination: Pagination
		},

	}
</script>

<style>
	.grid {
		width: 100%;
		max-width: 1280px;
		display: flex;
		justify-content: space-between;
		flex-wrap: wrap;
		margin: 40px auto 0;
	}

	.grid__item {
		width: 30%;
		flex-grow: 1;
		flex-shrink: 1;
		margin: 0 20px 40px;
	}

	/* Фотокарточки */
	.card {
		background-color: #fff;
		overflow: hidden;
		box-shadow: 0 1px 2px rgba(0, 0, 0, .2);
		border-radius: 2px;
		line-height: 0;
		cursor: pointer;
	}

	.card:hover {
		box-shadow: 0 3px 6px rgba(0, 0, 0, .2)
	}

	.card__body {
		width: 100%;
		height: 215px;
		overflow: hidden;
		background-color: #eee;
	}

	.card__body img {
		width: 100%;
		height: 100%;
		object-fit: cover;
	}

	.card__footer {
		width: 100%;
		padding: 10px 15px;
	}

	.media__obj {
		width: 32px;
		height: 32px;
		border-radius: 50%;
		background-color: #d8d8d8;
		margin-right: 15px;
		float: left;
	}

	.media__body {
		width: 100%;
		height: 32px;
		line-height: 32px;
	}

	.media__body a {
		font-family: Courier, serif;
		font-size: 15px;
		color: #999;
	}

	.media__body a:hover {
		text-decoration: none;
	}

	/* Пагинация */
	.pagination {
		width: 100%;
		height: 44px;
		display: flex;
		justify-content: space-between;
		margin: 30px auto 30px;
		padding: 0 15px;
		max-width: 1280px;
	}

	.pagination__left {
		float: left;
	}

	.pagination__right {
		float: right;
	}

	.pagination__right a {
		float: right;
	}

	.pagination a, .pagination span {
		display: block;
		text-align: center;
		font-family: Helvetica, Arial, sans-serif;
		font-weight: 300;
		line-height: 42px;
		height: 44px;
		color: #999;
		font-size: 18px;
	}

	.pagination a {
		padding: 0 13px;
		max-width: 160px;
		background-color: transparent;
		border-radius: 10px;
		border: 1px solid #ccc;
		text-decoration: none;
		margin: 0 6px;
		transition: all .2s ease-in-out;
	}

	.pagination a.current {
		border-color: #28a745;
		color: #28a745;
	}

	@media (hover) {
		.pagination a:hover {
			border-color: #28a745;
			color: #28a745;
		}
	}

	.pagination__mid {
		display: flex;
		justify-content: center;
		width: 60%;
	}

	.pagination__mid ul {
		list-style: none;
		padding: 0;
		margin: 0;
	}

	.pagination__mid li {
		display: inline-block;
	}
</style>
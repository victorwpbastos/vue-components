<template>
	<div class="row">
		<div class="col-sm-3">
			<div class="input-group">
				<select class="form-control" v-model="items_per_page" @change="paginate()">
					<option v-for="p in perPageOptions">{{ p }}</option>
				</select>
				<span class="input-group-addon">Registros por Página</span>
			</div>
		</div>
		<div class="col-sm-6">
			<div class="text-box">
				Exibindo {{ itemOffset }} a {{ offset_to }} de {{ itemCount }} {{ itemCount === 1 ? 'Registro' : 'Registros' }}
			</div>
		</div>
		<div class="col-sm-3 pull-right text-right">
			<div class="input-group">
				<div class="input-group-btn">
					<button type="button" class="btn btn-default" @click="paginate(parseInt(current_page, 10) - 1)" :disabled="current_page <= 1">
						<span class="glyphicon glyphicon-chevron-left"></span>
					</button>
				</div>
				<select class="form-control" v-model="current_page" @change="paginate(parseInt(current_page, 10))">
					<option v-for="p in pageCount" :value="p">Página {{ p }} de {{ pageCount }}</option>
				</select>
				<div class="input-group-btn">
					<button type="button" class="btn btn-default" @click="paginate(parseInt(current_page, 10) + 1)" :disabled="current_page >= pageCount">
						<span class="glyphicon glyphicon-chevron-right"></span>
					</button>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		props: {
			itemOffset: { default: 10 },
			itemCount: { default: 10 },
			pageCount: { default: 10 },
			currentPage: { default: 10 },
			itemsPerPage: {	default: 10	},
			perPageOptions: { default: () => [10, 25, 50, 100] },
		},

		data() {
			return {
				items_per_page: 10,
				current_page: 1
			}
		},

		created() {
			if (this.$route.query.items_per_page) {
				this.items_per_page = this.$route.query.items_per_page;
			}
		},

		watch: {
			currentPage(value) {
				this.current_page = value;
			}
		},

		computed: {
			offset_to() {
				if (this.itemCount <= this.itemsPerPage) {
					return this.itemCount;
				}

				return (this.itemOffset + this.itemsPerPage) - 1;
			}
		},

		methods: {
			paginate(page) {
				if (page) {
					this.current_page = page;
				}

				let query = Object.assign({}, this.$route.query, { page: this.current_page, items_per_page: this.items_per_page });

				this.$router.push({ query });
			}
		}
	}
</script>

<style scoped>
	.text-box {
		padding: 9px 12px;
		font-size: 14px;
		line-height: 1;
		color: #555;
		text-align: center;
	}
</style>
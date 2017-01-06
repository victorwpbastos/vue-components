<template>
	<div>
		<div class="panel panel-default" v-if="cols.length > 0">
			<div class="panel-heading">
				<div class="row">
					<div class="col-sm-6" :class="{ 'form-control-static': searcher }">
						<strong class="text-muted">{{title}}</strong>
					</div>
					<div class="col-sm-6" v-if="searcher">
						<v-searcher :search-fields="cols.map(c => c.split(':')[0])"></v-searcher>
					</div>
				</div>
			</div>
			<table class="table table-bordered table-hover" v-if="!isLoading">
				<thead>
					<tr>
						<slot name="header">
							<th class="text-center text-muted" :class="{ 'column-sort': sorter }" v-for="col in cols" :width="col.split(':')[1]" @click="orderBy(col.split(':')[0])">
								{{col.split(':')[0].toUpperCase()}}
								<span v-if="sorter">
									<template v-if="sortFields.column === col.split(':')[0]">
										<template v-if="sortFields.order === 'asc'">
											<span class="sort-icon pull-right fa fa-sort-up"></span>
										</template>
										<template v-else>
											<span class="sort-icon pull-right fa fa-sort-down"></span>
										</template>
									</template>
									<template v-else>
										<span class="sort-icon pull-right fa fa-unsorted"></span>
									</template>
								</span>
							</th>
						</slot>
					</tr>
				</thead>
				<tbody>
					<slot name="body" :items="itemList">
						<tr v-for="item in itemList">
							<td v-for="col in cols" v-html="item[col.split(':')[0]]"></td>
						</tr>
					</slot>
				</tbody>
			</table>
			<div class="panel-body text-center" v-else>
				<div class="alert alert-info" style="margin:0;">
					<i class="fa fa-spin fa-spinner"></i>
					<span>Carregando</span>
				</div>
			</div>
			<div class="panel-footer" v-if="paginator">
				<v-paginator :item-offset="itemOffset" :item-count="itemCount" :page-count="pageCount" :current-page="currentPage"></v-paginator>
			</div>
		</div>
		<div class="alert alert-warning" v-else>
			<strong>É obrigatório informar as colunas da tabela.</strong>
		</div>
	</div>
</template>

<script>
	import $ from 'jquery';
	import VPaginator from './paginator';
	import VSearcher from './searcher';

	export default {
		props: {
			title: { default: '' },
			source: { default: '' },
			cols: { default: () => [], required: true },
			searcher: { default: true },
			paginator: { default: true },
			sorter: { default: true }
		},

		components: { VPaginator, VSearcher },

		data() {
			return {
				isLoading: false,
				itemList: '',
				itemOffset: 1,
				itemCount: 1,
				pageCount: 1,
				currentPage: 1,
				sortFields: {
					column: '',
					order: 'asc'
				}
			}
		},

		created() {
			this.login(this.fetchData);

			if (this.$route.query.sort_fields) {
				let [column, order] = this.$route.query.sort_fields.split(':');

				this.sortFields = {column, order};
			}
		},

		watch: {
			'$route'() {
				this.fetchData();
			}
		},

		methods: {
			login(callback) {
				// let url = 'http://t.sistemas.sorocaba.sp.gov.br/PermisysManager_migracao/permisys';
				let url = `${Config.BASE_URL}/permisys`;
				let data = {
					usuario: 'testesistemas',
					senha: 'A12345678a'
				};

				$.post(url, data).then(response => callback(response)).catch(error => callback());
			},

			fetchData() {
				let timeout = setTimeout(() => this.isLoading = true, 500);

				$.get(this.source, this.$route.query).then(response => {
					clearTimeout(timeout);
					this.isLoading = false;

					this.itemList = response.itemList;
					this.itemOffset = response.itemOffset;
					this.itemCount = response.itemCount;
					this.pageCount = response.pageCount;
					this.currentPage = response.currentPage;
				});
			},

			orderBy(column) {
				if (this.sorter) {
					if (this.sortFields.column === column) {
						this.sortFields.order = this.sortFields.order === 'asc' ? 'desc' : 'asc';
					} else {
						this.sortFields.column = column;
						this.sortFields.order = 'asc';
					}


					let query = Object.assign({}, this.$route.query, { sort_fields: `${this.sortFields.column}:${this.sortFields.order}` });

					this.$router.push({ query });
				}
			}
		}
	}
</script>

<style scoped>
	/*table {
		width: 100%;
        table-layout: fixed;
	}*/

	.column-sort {
		cursor: pointer;
		user-select: none;
	}

	.sort-icon {
		margin-top: 3px;
	}

	th {
		white-space: nowrap;
	}
</style>
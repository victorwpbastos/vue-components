<template>
	<form @submit.prevent="search">
		<div class="input-group">
			<input type="text" class="form-control" v-model="searchValue">
			<div class="input-group-btn">
				<button type="submit" class="btn btn-default">
					<span class="glyphicon glyphicon-search"></span>
				</button>
			</div>
		</div>
	</form>
</template>

<script>
	export default {
		props: {
			searchFields: { default: () => [] }
		},

		data() {
			return {
				searchValue: ''
			}
		},

		created() {
			if (this.$route.query.fullsearch_value) {
				this.searchValue = this.$route.query.fullsearch_value;
			}
		},

		methods: {
			search() {
				let query = Object.assign({}, this.$route.query, { fullsearch_fields: this.searchFields.join(','), fullsearch_value: this.searchValue });

				if (this.searchValue === '') {
					delete query.fullsearch_fields;
					delete query.fullsearch_value;
				}

				this.$router.push({ query });
			}
		}
	}
</script>

<style scoped>

</style>
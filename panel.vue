<template>
	<div class="panel" :class="typeClass">
		<div class="panel-heading">{{ title }}</div>
		<slot></slot>
		<div class="panel-body" v-if="hasBody">
			<slot name="body"></slot>
		</div>
		<div class="panel-footer" v-if="hasFooter">
			<slot name="footer"></slot>
		</div>
	</div>
</template>

<script>
	export default {
		props: ['title', 'type'],

		data() {
			return {
				hasBody: false,
				hasFooter: false
			}
		},

		computed: {
			typeClass() {
				return `panel-${this.type || 'default'}`;
			}
		},

		mounted() {
			this.toggleSlots();
		},

		updated() {
			this.toggleSlots();
		},

		methods: {
			toggleSlots() {
				this.hasBody = this.$slots.body !== undefined && this.$slots.body !== null;
				this.hasFooter = this.$slots.footer !== undefined && this.$slots.footer !== null;
			}
		}
	}
</script>

<style scoped>
	.panel-info > .panel-footer {
		color: #31708f;
		background: #d9edf7;
		border-color: #bce8f1;
	}

	.panel-success > .panel-footer {
		color: #3c763d;
		background: #dff0d8;
		border-color: #d6e9c6;
	}

	.panel-warning > .panel-footer {
		color: #8a6d3b;
		background: #fcf8e3;
		border-color: #faebcc;
	}

	.panel-danger > .panel-footer {
		color: #a94442;
		background: #f2dede;
		border-color: #ebccd1;
	}
</style>
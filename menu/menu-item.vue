<template>
	<router-link tag="li" :event="event" :to="path" active-class="active" :class="{ 'active': active === '' }">
		<a :style="isActive ? activeStyle : ''"><span :class="icon"></span> <slot></slot></a>
	</router-link>
</template>

<script>
	export default {
		// props: ['path', 'icon', 'active', 'activeColor'],
		props: {
			path: { default: '' },
			icon: { default: '' },
			active: { default: false },
			activeColor: { default: 'salmon' },
			event: { default: 'click' }
		},

		computed: {
			activeStyle() {
				return {
					color: this.activeColor,
					borderBottomColor: this.activeColor
				}
			},

			isActive() {
				return this.active === '' || this.$route.path === this.path;
			}
		}
	}
</script>

<style scoped>
	li {
		float: left;
	}

	li, li > a {
		display: block;
		position: relative;
	}

	li > a {
		text-decoration: none;
		padding: 10px 15px;
		border-top: solid 3px transparent;
		border-bottom: solid 3px transparent;
		width: 100%;
	}

	li > a:active,
	li > a:focus,
	li > a:hover {
		color: #337ab7;
		background: #f7f7f7;
	}

	.menu-justified > li {
		float: none;
		display: table-cell;
		width: 1%;
		text-align: center;
	}
</style>
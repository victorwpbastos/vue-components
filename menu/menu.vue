<template>
	<ul :class="{ 'menu-justified': justified === '' }">
		<slot></slot>
	</ul>
</template>

<script>
	export default {
		props: ['justified']
	}
</script>

<style scoped>
	ul {
		width: 100%;
		padding: 0;
		margin: 0;
		list-style: none;
		display: block;
	}

	ul:after {
		display: block;
		content: '';
		clear: both;
	}
</style>
<template>
	<Cube />
</template>

<script>
import Cube from './components/Cube.vue';

export default {
	name: 'App',

	components: {
		Cube,
	},
};
</script>

<style>
#app {
	min-height: 100vh;

	display: flex;
	justify-content: center;
	align-items: center;
}
</style>

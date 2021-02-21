<template>
	<div class="side" :style="{ backgroundColor: bg, color: color }">
		<button @click="$emit('handleClick')" class="button">{{ name }}</button>

		<div class="colorInfo">
			<span class="colorText">{{ bg }}</span>
			
			<font-awesome-icon
				@click="copyColor(bg)"
				:color="color"
				class="copyIcon"
				icon="copy"
			/>
		</div>
	</div>
</template>

<script>
export default {
	props: ['color', 'bg', 'name'],

	methods: {
		// Работает только на https
		copyColor(text) {
			navigator.clipboard.writeText(text);
		}
	}
}
</script>

<style>
.side {
	width: 300px;
	height: 300px;

	position: absolute;

	transform-origin: center center -150px;

	display: flex;
	justify-content: center;
	align-items: center;
	flex-direction: column;

	box-shadow: 0px 0px 100px 0px rgba(34, 60, 80, 0.15) inset;
}

.button {
	border: 0;
	outline: none;

	background: transparent;
	
	font-family: 'Arial', sans-serif;
	font-size: 2em;
	color: inherit;
	
	cursor: pointer;
}

.colorInfo {
	display: flex;
	align-items: center;

	font-family: 'Courier New', Courier, monospace;
	font-size: 1.2em;
	margin-top: 15px;
}

.copyIcon {
	cursor: pointer;
	margin-left: 5px;
}
</style>
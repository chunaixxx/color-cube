<template>
	<div class="basePopup">
		<slot></slot>
	</div>
</template>

<style>
.basePopup {
	display: inline-block;

	background: #222;
	color: #fff;
	font-family: 'Arial', sans-serif;
	padding: 5px 10px;
	border-radius: 5px;
	letter-spacing: 1px;
}

.basePopup:before {
	content: '';
	position: absolute;
	bottom: -4px;
	left: 50%;
	transform: translateX(-50%);

	border-left: 12px solid transparent;
	border-right: 12px solid transparent;
	border-top: 5px solid #222;
}
</style>
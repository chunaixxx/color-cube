<template>
	<div class="cube">
		<transition name="popup-fade">
			<BasePopup class="popup" v-if="showPopup">click to flip</BasePopup>
		</transition>

		<transition 
			name="side-rotate"
			enter-active-class="side-active"
			leave-active-class="side-active"
			leave-to-class="side-out"
			enter-from-class="side-in"
		>
			<CubeSide 
				v-if="toggle"

				@handleClick="showNewSide"
				v-bind:="side"				
				key="side-1"
			/>

			<CubeSide 
				v-else

				@handleClick="showNewSide"
				v-bind:="side"				
				key="side-2"
			/>
		</transition>
	</div>
</template>

<script>
import BasePopup from "./BasePopup.vue";
import CubeSide from './CubeSide.vue'

export default {
	name: 'Cube',
	components: { CubeSide, BasePopup },

	data: () => ({
		toggle: true,
		showPopup: true,

		side: {
			color: '#ffffff',
			bg: '#000000',
			name: 'Black'
		}
	}),

	methods: {
		// Возвращает случайный цвет
		getRandomColor() {
			return '#' + Math.random().toString(16).slice(2, 8);
		},

		// Возвращает констрастный цвет (серый или белый)
		getContrastColor(hex) {
			const r = parseInt(hex.substr(1, 2), 16);
			const g = parseInt(hex.substr(3, 2), 16);
			const b = parseInt(hex.substr(5, 2), 16);

			const yiq = (r * 299 + g * 587 + b * 114) / 1000;

			return yiq >= 170 ? '#222' : '#fff';
		},

		// Возвращает название цвета с помощью API
		async getNameColor(hex) {
			let nameColor;

			try {
				const res = await fetch(`https://www.thecolorapi.com/id?hex=${ hex.slice(1) }`);
				nameColor = (await res.json()).name.value;
			} catch(e) {
				nameColor = 'No name';
			}

			return nameColor;
		},

		// Генерирурет новые данные для стороны куба и запускает анимацию для ее показа
		async showNewSide() {
			this.showPopup = false;

			const randomColor = this.getRandomColor();
			const nameColor = await this.getNameColor(randomColor);

			this.side = {
				color: this.getContrastColor(randomColor),
				bg: randomColor,
				name: nameColor
			};

			// Запускает анимацию с помощью vue transition
			this.toggle = !this.toggle;
		},
	},
};
</script>

<style scoped>
.cube {
	width: 300px;
	height: 300px;

	position: relative;
	transform-style: preserve-3d;
}

.popup {
	position: absolute;
	left: 50%;
	top: 50px;
	z-index: 2;

	animation: popup-anim 1s ease-in infinite alternate;
}

@keyframes popup-anim {
	from {
		transform: translateX(-50%);
	}

	to {
		transform: translateX(-50%) translateY(10px);
	}
}

/* Vue transition */
.side-active {
	transition: all .4s ease-in-out;
}

.side-out {
	transform: rotateX(90deg);
}

.side-in {
	transform: rotateX(-90deg);
}

.popup-fade-enter-active,
.popup-fade-leave-active {
	transition: all .3s ease;
}

.popup-fade-enter-from,
.popup-fade-leave-to {
	opacity: 0;
}
/* */
</style>

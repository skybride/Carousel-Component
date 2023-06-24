<template>
	<div class="carousel">
		<div class="carousel-container">
			<div
				class="slide"
				v-for="(item, index) in items"
				:key="index"
				:style="{ transform: `translateX(${(index - currentIndex) * 100}%)` }"
			>
				<img :src="item.image" :alt="item.caption" />
				<div class="caption">{{ item.caption }}</div>
			</div>
		</div>

		<button
			class="prev-button"
			@click="goToPreviousSlide"
			:disabled="currentIndex === 0"
		>
			Previous
		</button>
		<button
			class="next-button"
			@click="goToNextSlide"
			:disabled="currentIndex === items.length - 1"
		>
			Next
		</button>
	</div>
</template>

<script lang="ts">
import { defineComponent, PropType, ref } from "vue";

interface CarouselItem {
	image: string;
	caption: string;
}

export default defineComponent({
	name: "Carousel",
	props: {
		items: {
			type: Array as PropType<CarouselItem[]>,
			required: true,
		},
	},
	setup(props) {
		const currentIndex = ref(0);

		const goToPreviousSlide = () => {
			if (currentIndex.value > 0) {
				currentIndex.value--;
			}
		};

		const goToNextSlide = () => {
			if (currentIndex.value < props.items.length - 1) {
				currentIndex.value++;
			}
		};

		return {
			currentIndex,
			goToPreviousSlide,
			goToNextSlide,
		};
	},
});
</script>

<style scoped>
.carousel {
	position: relative;
	overflow: hidden;
}

.carousel-container {
	display: flex;
	width: 100%;
	transition: transform 0.5s;
	flex-shrink: 0;
}

.slide {
	flex: 0 0 100%;
	width: 100%;
}

img {
	width: 100%;
	height: auto;
	object-fit: cover;
}

.caption {
	background-color: #000;
	color: #fff;
	padding: 10px;
	text-align: center;
}

.prev-button,
.next-button {
	position: absolute;
	top: 50%;
	transform: translateY(-50%);
	padding: 10px;
	font-size: 16px;
	background-color: #fff;
	border: none;
	cursor: pointer;
}

.prev-button {
	left: 10px;
}

.next-button {
	right: 10px;
}
</style>

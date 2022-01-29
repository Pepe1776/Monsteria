<template>
	<div class="container bg-gradient-to-r from-green-500 via-blue-500 to-green-900">
		<div
				id="flippable-carousel-title"
				class="absolute left-1/2 transform -translate-x-1/2 top-3"
			>
					<div class="text-7xl lg:text-9xl neon-title whitespace-nowrap">
					Monsteria
				</div>
			</div>
		<Carousel />
		<Pheno />
	</div>
</template>
<script lang="ts">
import Carousel from './components/Carousel.vue'
import Pheno from './components/Pheno.vue'
export default {
	components: {
		Carousel,
		Pheno,
	}
}
</script>
<style lang="scss">
@import '@/styles/mixin.scss';
	.neon-title {
	text-shadow: 0 0 3vw #23ff23;

	animation: flux 2s linear infinite;
	-moz-animation: flux 2s linear infinite;
	-webkit-animation: flux 2s linear infinite;
	-o-animation: flux 2s linear infinite;
}

#flippable-carousel {
	&-title {
		font-family: 'Vibur', cursive;
	}

	&-flip {
		@include mobile {
			height: $card-size;
		}
		@include desktop {
			height: $card-size * 1.4;
		}
	}

	&-navigator {
		@include mobile {
			width: $mobile-size * 1.3;
		}
		@include desktop {
			width: $card-size * 1.3;
		}
	}
}
</style>
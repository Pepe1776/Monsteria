<template>
	<section
		class="w-full h-screen flex justify-center items-start  "
	>
		<article class="w-full h-screen">
		
			<div
				v-if="flippableCardInfos && flippableCardInfos.length"
				id="flippable-carousel"
				ref="flippableCarousel"
				class="w-full h-screen flex items-top"
			>
				<!-- SECTION cards -->
				<div
					id="flippable-carousel-flip"
					class="w-full absolute overflow-hidden flex items-center mt-2"
				>
					<FlippableCard
						v-for="(card, cardIndex) in flippableCardInfos"
						:key="cardIndex"
						:card="card"
						:cardIndex="cardIndex"
						:slideToSelectedCard="slideToSelectedCard"
					/>
				</div>

				<!-- SECTION navigation -->
				<div
					id="flippable-carousel-navigator"
					class="absolute left-1/2 transform -translate-x-1/2 z-50 flex items-top h-card-mobile lg:h-card-web mt-28"
					v-touch:swipe.lefe="(e) => slideToSelectedCard(e, 'NEXT')"
					v-touch:swipe.right="(e) => slideToSelectedCard(e, 'PREV')"
				>
					<div :class="`w-full justify-between ${isSliding ? 'hidden' : 'flex'}`">
						<button @click="(e) => slideToSelectedCard(e, 'PREV')" class="w-6 transform rotate-180">
							<CarouselArrow />
						</button>
						<button @click="(e) => slideToSelectedCard(e, 'NEXT')" class="w-6">
							<CarouselArrow />
						</button>
					</div>
					<div
						id="flippable-carousel-navigator-fakeface"
						class="absolute left-1/2 transform -translate-x-1/2 w-card-mobile h-card-mobile lg:w-card-web lg:h-card-web"
						@mouseenter.prevent="() => flipCardHandler('ENTER')"
						@mouseleave.prevent="() => flipCardHandler('LEAVE')"
						v-touch:tap="() => flipCardHandler('ENTER')"
						v-touch:end="() => flipCardHandler('LEAVE')"
					></div>
				</div>
			</div>
		</article>
	</section>
</template>

<script lang="ts">
import Vue, { VueConstructor } from 'vue';
import { FlippableCard } from '@/components';
import { CarouselArrow } from '@/assets';
import { FlippableCardInfo, NullableElement } from '@/typings';

interface LocalTypes {
	flippableCardInfos: FlippableCardInfo[];
	slideToSelectedCard: () => void;
	flipCardHandler: () => void;
	slideCardByKey: () => void;
	hideNavigationWhileSliding: () => void;
}

export default (Vue as VueConstructor<Vue & LocalTypes>).extend({
	name: 'App',

	components: {
		CarouselArrow,
		FlippableCard,
	},

	data: () => ({
		isSliding: false as boolean,
		flippableCardInfos: [
			{
				frontImage: 'https://cdn.pixabay.com/photo/2022/01/16/10/51/leaves-6941709_1280.jpg',
				selectedStatus: ['prev-second'],
			},
			{
				frontImage: 'https://cdn.pixabay.com/photo/2019/11/24/05/22/green-4648611_1280.jpg',
				selectedStatus: ['prev'],
			},
			{
				frontImage: 'https://cdn.pixabay.com/photo/2019/05/17/07/14/palm-4209050_1280.jpg',
				selectedStatus: ['selected'],
			},
			{
				frontImage: 'https://cdn.pixabay.com/photo/2021/06/08/02/32/monstera-6319467_1280.jpg',
				selectedStatus: ['next'],
			},
			{
				frontImage: 'https://cdn.pixabay.com/photo/2019/02/06/15/58/monstera-3979429_1280.jpg',
				selectedStatus: ['next-second'],
			},
		],
	}),

	created() {
		document.addEventListener('keydown', this.slideCardByKey);
		this.initConsole();
	},

	mounted() {
		this.hideNavigationWhileSliding();
	},

	destroyed() {
		document.removeEventListener('keydown', this.slideCardByKey);
	},

	methods: {
		slideToSelectedCard(e: MouseEvent | PointerEvent | null, clickedButton: string) {
			// flipback
			this.flipCardHandler('LEAVE');

			const directlySelected = e?.currentTarget as HTMLDivElement;
			const currentSelected = document.querySelector('.selected');
			const currentNext = currentSelected?.nextElementSibling;
			const currentPrev = currentSelected?.previousElementSibling;

			let selected: NullableElement;

			switch (clickedButton?.toUpperCase()) {
				case 'NEXT':
					selected = currentNext;
					break;
				case 'PREV':
					selected = currentPrev;
					break;
				default:
					selected = directlySelected;
					break;
			}

			try {
				if (!selected) throw new Error('No More Selectable Card');

				let nextCard: NullableElement = selected.nextElementSibling;
				let prevCard: NullableElement = selected.previousElementSibling;
				let nextSecondCard: NullableElement = nextCard?.nextElementSibling;
				let prevSecondCard: NullableElement = prevCard?.previousElementSibling;

				// rotating to current selected
				const cardConditions = ['selected', 'prev', 'next', 'next-second', 'prev-second'];

				selected.classList.remove(...cardConditions);
				selected.classList.add('selected');

				nextCard?.classList.remove(...cardConditions);
				nextCard?.classList.add('next');

				prevCard?.classList.remove(...cardConditions);
				prevCard?.classList.add('prev');

				nextSecondCard?.classList.remove(...cardConditions);
				nextSecondCard?.classList.add('next-second');

				prevSecondCard?.classList.remove(...cardConditions);
				prevSecondCard?.classList.add('prev-second');
			} catch (error) {
				console.error(error.message);
			}
		},

		flipCardHandler(status: string) {
			const currentSelected = document.querySelector('.selected');
			const frontFace = currentSelected?.children[0] as HTMLDivElement;
			const backFace = currentSelected?.children[1] as HTMLDivElement;

			switch (status.toUpperCase()) {
				case 'ENTER':
					frontFace.classList.add('card-flip-front');
					backFace.classList.remove('card-flip-back');
					break;
				case 'LEAVE':
					frontFace.classList.remove('card-flip-front');
					backFace.classList.add('card-flip-back');
					break;

				default:
					break;
			}
		},

		slideCardByKey(e: KeyboardEvent) {
			switch (e.code.toUpperCase()) {
				case 'ARROWLEFT':
					this.slideToSelectedCard(null, 'prev');
					break;
				case 'ARROWRIGHT':
					this.slideToSelectedCard(null, 'next');
					break;
				case 'ARROWUP':
					this.flipCardHandler('ENTER');
					break;
				case 'ARROWDOWN':
					this.flipCardHandler('LEAVE');
					break;

				default:
					break;
			}
		},

		hideNavigationWhileSliding() {
			const flippableCarousel = this.$refs.flippableCarousel as HTMLDivElement;
			flippableCarousel.ontransitionrun = () => (this.isSliding = true);
			flippableCarousel.ontransitionstart = () => (this.isSliding = true);
			flippableCarousel.ontransitionend = () => (this.isSliding = false);
		},

		initConsole() {
			const consoleStyle = `
				font-family: -apple-system, sans-serif;
				font-size: 2.5em; 
				font-weight: 300;
				border-radius: 5px;
				padding: 15px;
				background: #1D222B;
				color: #fff;
				text-shadow:
						0 0 7px #bc13fe,
						0 0 10px #B4F8C8,
						0 0 21px #A0E7E5
			`;

			console.info('%c  Github.com/Pepe1776  ', consoleStyle);
		},
	},
});
</script>

<style lang="scss" scoped>
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

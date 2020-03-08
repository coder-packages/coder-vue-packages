<template>
	<div ref="button" class="copy-button" @click="copy">
		<slot></slot>
		<transition name="slide-up">
			<div v-show="show" class="copy-button__popup">
				<slot name="popup"></slot>
			</div>
		</transition>
	</div>
</template>
<script>
	export default {
		props: ['target'],
		data() {
			return {
				show: false
			};
		},
		methods: {
			copy() { // string or input element
				const el = document.createElement('textarea');
				el.value = typeof this.target == 'string' ? this.target : this.target.value;
				document.body.appendChild(el);
				el.select();
				document.execCommand('copy');
				document.body.removeChild(el);
				this.show = true;
				setTimeout(() => {
					this.show = false;
				}, 2000);
			}
		}
	}
</script>

<style lang="scss">
	.copy-button {
		cursor: pointer;
	}

	.slide-up-enter-active, .slide-up-leave-active {
		transition: transform .2s;
	}

	.slide-up-enter, .slide-up-leave-to {
		opacity: 0;
		transform: translateX(-50%) translateY(10px);
	}
</style>

<script>
	export default {
		props: ['handler'],
		created() {
			const originalTitle = document.title;

			document.addEventListener("visibilitychange", () => {
				if (this.handler) {
					this.handler();
				} else {
					document.title = document.visibilityState === 'visible' ? originalTitle : this.$slots.default[0].text;
				}
			});
		},
		render() {
			return null;
		}
	}
</script>

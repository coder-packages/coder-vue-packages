<script>
	import ElementResizeDetector from 'element-resize-detector';

	const erd = ElementResizeDetector({strategy: 'scroll'});

	export default {
		data() {
			return {
				width: this.width,
				height: this.height
			}
		},
		mounted() {
			erd.listenTo(this.$el, el => {
				this.width = el.offsetWidth;
				this.height = el.offsetHeight;
			});
		},
		render() {
			return this.$scopedSlots.default({
				width: this.width,
				height: this.height
			})
		}
	}
</script>

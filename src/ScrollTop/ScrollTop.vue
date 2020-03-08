<template>
  <transition name="fade">
    <button
      ref="button"
      type="button"
      class="scroll-top"
      @click="topTop"
      v-show="show"
    >
      <slot></slot>
    </button>
  </transition>
</template>

<script>
export default {
  props: {
    showAfter: {
      type: Number,
      default: 100
    }
  },
  data() {
    return {
      show: false
    };
  },
  created() {
    const handler = e => (this.show = window.scrollY > this.showAfter);
    window.addEventListener("scroll", handler);
    this.$once("hook:destroyed", () => {
      window.removeEventListener("scroll", handler);
    });
  },
  methods: {
    topTop() {
      /*document.querySelector('body').scrollIntoView({
					behavior: 'smooth'
				}); // doesn't support on some browsers */
      window.scroll({ top: 0, behavior: "smooth" });
    }
  }
};
</script>

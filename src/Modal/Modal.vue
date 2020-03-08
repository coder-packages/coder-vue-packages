<template>
  <portal to="modals">
    <div class="c-modal">
      <div class="c-modal__backdrop" v-show="show && backdrop"></div>
      <transition name="modal">
        <div class="c-modal__scroller" tabindex="-1" v-show="show">
          <div class="c-modal__wrapper">
            <click-outside :do="handleClickOutside">
              <div class="c-modal__container">
                <slot></slot>
              </div>
            </click-outside>
          </div>
        </div>
      </transition>
    </div>
  </portal>
</template>

<script>
import ClickOutside from "../ClickOutside/ClickOutside";

export default {
  components: {
    ClickOutside
  },
  model: {
    prop: "show",
    event: "input"
  },
  props: {
    show: {
      type: Boolean,
      default: false
    },
    backdrop: {
      type: Boolean,
      default: true
    },
    closeOnClickOutside: {
      type: Boolean,
      default: true
    },
    closeOnEsc: {
      type: Boolean,
      default: true
    }
  },
  created() {
    const handler = e => {
      if (this.closeOnEsc && e.key === "Escape" && this.show) {
        this.cancel();
      }
    };

    document.addEventListener("keydown", handler);
    this.$once("hook:destroyed", () => {
      document.removeEventListener("keydown", handler);
    });
  },
  methods: {
    cancel() {
      this.$emit("input", false);
    },
    handleClickOutside() {
      if (this.closeOnClickOutside) {
        this.cancel();
      }
    }
  },
  watch: {
    show: {
      immediate: true,
      handler(show) {
        if (show) {
          this.$nextTick(() => {
            document.body.style.setProperty("overflow", "hidden");
            document.body.style.setProperty("max-height", "100vh");
          });
        } else {
          document.body.style.removeProperty("overflow");
          document.body.style.removeProperty("max-height");
        }
      }
    }
  }
};
</script>

<style lang="scss" src="./Modal.scss"></style>

<template>
	<click-outside :do="close">
		<div
			class="cs-container"
			:class="{'cs-container--active': isOpen}"
			@keydown.esc="close"
			@keydown.up="highlightPrev"
			@keydown.down="highlightNext"
			@keydown.enter.prevent="selectHighlighted"
			@keydown.tab.prevent
		>
			<button type="button" class="cs-button" @click="open" ref="button">
				<span class="cs-button__text" v-if="computedValue">{{ value instanceof Object ? value.text : value }}</span>
				<span class="cs-button__placeholder" v-else>{{ computedPlaceholder }}</span>
				<span class="cs-button__arrow"></span>
			</button>

			<div class="cs-dropdown" v-if="isOpen" ref="dropdown">
				<input class="cs-search" v-model="search" ref="search" v-if="searchable"/>
				<ul class="cs-options" v-if="filteredOptions.length > 0" ref="options">
					<li
						class="cs-option"
						:class="{'cs-option--highlighted': highlightedIndex === i}"
						v-for="(option, i) in filteredOptions"
						:key="i"
						@click="select(option)"
					>{{ option instanceof Object ? option.text : option }}
					</li>
				</ul>
				<div class="cs-empty" v-else>{{ empty }}</div>
			</div>
		</div>
	</click-outside>
</template>

<script>
	import Popper from 'popper.js';
	import ClickOutside from "@/vue-components/click-outside";

	export default {
		components: {
			ClickOutside
		},
		props: {
			value: {required: true},
			placeholder: {default: "Choose ..."},
			empty: {default: "No results"},
			options: {type: Array},
			filterFunction: {default: null},
			searchable: {type: Boolean, default: false}
		},
		data() {
			return {
				isOpen: false,
				search: "",
				highlightedIndex: 0,
			};
		},
		created() {
			if (!this.placeholder && !this.value && this.options.length) {
				this.$emit('input', this.options[0]);
			}
		},
		beforeDestroy() {
			if (this.popper !== undefined) {
				this.popper.destroy();
			}
		},
		computed: {
			filteredOptions() {
				if (this.filterFunction === null) {
					return this.options.filter(option => {
						const _option = option instanceof Object ? option.text : option;
						return _option.toLowerCase().startsWith(this.search.toLowerCase());
					});
				} else {
					return this.filterFunction(this.search, this.options);
				}
			},
			computedValue() {
				return this.value instanceof Object ? this.value.value : this.value;
			},
			computedPlaceholder() {
				if (this.value instanceof Object) {
					return this.value.text;
				} else {
					if (this.placeholder) {
						return this.placeholder;
					} else {
						return this.options[0] instanceof Object ? this.options[0].text : this.options[0];
					}
				}
			}
		},
		methods: {
			open() {
				if (this.isOpen) {
					this.close();
					return;
				}
				this.isOpen = true;
				this.$nextTick(() => {
					this.setupPopper();
					if (this.searchable) {
						this.$refs.search.focus();
					}
					this.scrollToHighlighted();
				});
			},
			close() {
				if (!this.isOpen) {
					return;
				}
				this.isOpen = false;
				this.search = '';
				if (this.popper !== undefined) {
					this.popper.destroy();
				}
			},
			select(option) {
				this.$emit("input", option);
				this.search = '';
				this.highlightedIndex = 0;
				this.close();
			},
			highlight(index) {
				this.highlightedIndex = index;

				if (this.highlightedIndex < 0) {
					this.highlightedIndex = this.filteredOptions.length - 1;
				}

				if (this.highlightedIndex > this.filteredOptions.length - 1) {
					this.highlightedIndex = 0;
				}

				this.scrollToHighlighted();
			},
			highlightPrev() {
				this.highlight(this.highlightedIndex - 1);
			},
			highlightNext() {
				this.highlight(this.highlightedIndex + 1);
			},
			selectHighlighted() {
				if (this.filteredOptions[this.highlightedIndex] !== undefined) {
					this.select(this.filteredOptions[this.highlightedIndex]);
				}
			},
			scrollToHighlighted() {
				this.$refs.options.children[this.highlightedIndex].scrollIntoView({
					block: "nearest"
				});
			},
			setupPopper() {
				this.popper = new Popper(this.$refs.button, this.$refs.dropdown, {
					placement: 'bottom'
				});
			}
		},
		watch: {
			search() {
				this.popper.update();
			}
		}
	};
</script>


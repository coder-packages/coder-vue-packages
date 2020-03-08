<template>
	<div class="form-field" :class="{'form-field--error': error}">
		<slot name="default">
			<component :is="inputProps.field"
								 v-bind="inputProps"
								 class="form-field__input"
								 :placeholder="computedPlaceholder"
								 :value="value"
								 @input="$emit('input', $event.target.value)"
			/>
		</slot>
		<slot name="label" v-if="showLabel"><label class="form-field__label">{{ label }}</label></slot>
		<div class="error form-field__error" v-if="error">
			<i></i><span>{{ formattedErrorMessage }}</span>
		</div>
	</div>
</template>

<script>
	export default {
		inject: {
			globalErrorMessage: 'errorMessage'
		},
		props: {
			value: {
				type: String,
				default: ''
			},
			label: {
				type: String
			},
			error: {
				default: false
			},
			errorMessage: {
				type: String,
				value: ''
			},
			inputProps: {
				type: Object,
				default() {
					return {
						field: 'input',
						type: 'text',
					}
				}
			}
		},
		computed: {
			showLabel() {
				return this.label || this.$slots.label;
			},
			computedPlaceholder() {
				return this.inputProps.placeholder || this.label;
			},
			formattedErrorMessage() {
				const string = this.errorMessage || this.globalErrorMessage;
				return string.replace('%s', this.label);
			}
		}
	}
</script>

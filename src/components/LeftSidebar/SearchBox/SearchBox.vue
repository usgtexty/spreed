<!--
  - @copyright Copyright (c) 2019 Marco Ambrosini <marcoambrosini@icloud.com>
  -
  - @author Marco Ambrosini <marcoambrosini@icloud.com>
  -
  - @license GNU AGPL version 3 or any later version
  -
  - This program is free software: you can redistribute it and/or modify
  - it under the terms of the GNU Affero General Public License as
  - published by the Free Software Foundation, either version 3 of the
  - License, or (at your option) any later version.
  -
  - This program is distributed in the hope that it will be useful,
  - but WITHOUT ANY WARRANTY; without even the implied warranty of
  - MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
  - GNU Affero General Public License for more details.
  -
  - You should have received a copy of the GNU Affero General Public License
  - along with this program. If not, see <http://www.gnu.org/licenses/>.
-->

<template>
	<NcTextField ref="searchConversations"
		:value="value"
		:label="placeholderText"
		:show-trailing-button="isFocused"
		trailing-button-icon="close"
		v-on="listeners"
		@blur="handleBlur"
		@update:value="updateValue"
		@trailing-button-click="abortSearch"
		@keydown.esc="abortSearch">
		<Magnify :size="16" />
	</NcTextField>
</template>

<script>
import Magnify from 'vue-material-design-icons/Magnify.vue'

import NcTextField from '@nextcloud/vue/dist/Components/NcTextField.js'

export default {
	name: 'SearchBox',
	components: {
		NcTextField,
		Magnify,
	},
	props: {
		/**
		 * The placeholder for the input field
		 */
		placeholderText: {
			type: String,
			default: t('spreed', 'Search …'),
		},
		/**
		 * The value of the input field.
		 */
		value: {
			type: String,
			required: true,
		},
		/**
		 * If true, this component displays an 'x' button to abort the search
		 */
		isFocused: {
			type: Boolean,
			default: false,
		},
	},

	expose: ['focus'],

	emits: ['update:value', 'input', 'submit', 'abort-search', 'blur', 'trailing-blur'],

	computed: {
		listeners() {
			return Object.assign({}, this.$listeners, {
				blur: this.handleBlur,
			})
		},

		cancelSearchLabel() {
			return t('spreed', 'Cancel search')
		},
	},

	methods: {
		updateValue(value) {
			this.$emit('update:value', value)
			this.$emit('input', value)
		},
		// Focuses the input.
		focus() {
			this.$refs.searchConversations.focus()
		},
		/**
		 * Emits the abort-search event and re-focuses the input
		 */
		abortSearch() {
			this.$emit('abort-search')
		},

		handleBlur(event) {
			if ((event.relatedTarget) && (Array.from(event.relatedTarget.classList).includes('input-field__clear-button'))) {
				event.preventDefault()
				this.$refs.searchConversations.$el.querySelector('.input-field__clear-button').addEventListener('blur', (event) => {
					this.$emit('trailing-blur', event)
				})
			} else {
				this.$emit('blur', event)
			}
		},

	},
}
</script>

<style lang="scss" scoped>

:deep(.input-field__input) {
	border-radius: var(--border-radius-pill);
}

</style>

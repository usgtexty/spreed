<template>
	<div class="form">
		<div class="user-list">
			<strong>Recipients:</strong> {{ this.getRecipients() }}
		</div>
		<div>
			<label for="emailFormSubject"></label>
			<input type="text"
				placeholder="Meeting subject..."
				class="message-subject"
				id="emailFormSubject"
				:value="subject"
				@input="handleSubjectInput">
		</div>
		<div>
			<label for="emailFormMessage">Message</label>
			<textarea v-model="template"
				placeholder="Enter your message"
				class="message-body"
				id="emailFormMessage"
				@input="handleBodyInput"></textarea>
		</div>
	</div>
</template>

<script>
export default {
	name: 'EmailForm',
	props: {
		recipients: {
			type: Array,
			required: true,
		},
		subject: {
			type: String,
			required: true
		},
		body: {
			type: String,
			required: true,
		},
	},

	data() {
		return {
			template: ''
		}
	},

	mounted() {
		this.template = this.body
	},

	methods: {
		getRecipients() {
			const length = this.recipients.length
			const recipients = this.recipients.map(r => r.label)

			if (length > 3) {
				let text = recipients.slice(0, 3).join(', ')
				text += ', and ' + (length - 3) + ' more...'

				return text
			} else {
				return recipients.join(', ')
			}
		},

		handleBodyInput() {
			this.$emit('handle-body', event.target.value)
		},

		handleSubjectInput() {
			this.$emit('handle-subject', event.target.value)
		},
	}
}
</script>

<style lang="scss" scoped>
	.message-subject,
	.message-body {
		width: 100%;
	}

	.message-body {
		height: 130px;
	}
</style>

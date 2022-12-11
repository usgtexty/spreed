<template>
  <div class="form">
    <div class="user-list">
      <strong>Recipients:</strong> {{ getRecipients() }}
    </div>
    <div>
      <label for="emailFormSubject">Subject</label>
      <input id="emailFormSubject"
        :value="subject"
        class="message-subject"
        type="text"
        placeholder="Meeting subject..."
        @input="handleSubjectInput" />
    </div>
    <div>
      <label for="emailFormMessage">Message</label>
      <textarea id="emailFormMessage"
        v-model="template"
        class="message-body"
        placeholder="Enter your message"
        @input="handleBodyInput" />
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
      required: true,
    },
    body: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      template: '',
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
  },
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

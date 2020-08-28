<template>
  <q-btn color="primary" icon="content_copy" unelevated round @click="copy" dense/>
</template>

<script>
import { copyToClipboard } from 'quasar'

export default {
  name: 'ScevaCopyButton',
  props: {
    text: {
      type: String,
      required: true
    }
  },
  methods: {
    copy () {
      const self = this
      copyToClipboard(this.text)
        .then(() => {
          self.$app.notify(`${self.text} copied!`)
        })
        .catch(() => {
        })
    }
  }
}
</script>

<style scoped>

</style>

<template>
  <q-select outlined dense v-model="appId" :options="apps" label="App *" lazy-rules
            :rules="[ val => !!val || 'App is required']" use-chips :disable="loading"
            option-label="name" option-value="id" emit-value map-options/>
</template>

<script>
import { mapState } from 'vuex'

export default {
  name: 'ScevaAppSelect',
  props: {
    value: {
      required: true
    },
    loading: {
      type: Boolean,
      required: false,
      default: false
    }
  },
  data () {
    return {
      appId: null
    }
  },
  watch: {
    appId (val) {
      this.$emit('input', val)
    }
  },
  computed: {
    ...mapState('app', {
      apps: state => state.apps
    })
  },
  mounted () {
    this.appId = this.value
  }
}
</script>

<style scoped>

</style>

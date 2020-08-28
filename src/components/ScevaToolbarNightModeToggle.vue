<template>
  <q-toggle v-model="darkMode" checked-icon="invert_colors" unchecked-icon="invert_colors_off" size="lg"
            keep-color/>
</template>

<script>
export default {
  name: 'ScevaToolbarNightModeToggle',
  data () {
    return {
      darkMode: this.$q.dark.isActive
    }
  },
  watch: {
    '$q.dark.isActive' (val) {
      // this.$app.notifyFormError(val ? 'On dark mode' : 'On light mode')
      this.darkMode = val
    },
    darkMode (val) {
      this.$q.dark.set(val)
    }
  }
}
</script>

<style scoped>

</style>

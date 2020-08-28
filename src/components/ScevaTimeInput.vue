<template>
  <q-input outlined dense v-model="time" mask="time" label="Time" :disable="loading">
    <template v-slot:append>
      <q-icon name="access_time" class="cursor-pointer">
        <q-popup-proxy transition-show="scale" transition-hide="scale">
          <q-time v-model="time" mask="HH:mm" format24h>
            <div class="row items-center justify-end q-gutter-sm">
              <q-btn label="Cancel" color="primary" flat v-close-popup/>
              <q-btn label="OK" color="primary" flat v-close-popup/>
            </div>
          </q-time>
        </q-popup-proxy>
      </q-icon>
    </template>
  </q-input>
</template>

<script>
export default {
  name: 'ScevaTimeInput',
  props: {
    value: {
    },
    loading: {
      type: Boolean,
      required: false,
      default: false
    }
  },
  data () {
    return {
      time: null
    }
  },
  watch: {
    time (val) {
      this.$emit('input', `${val}:00`)
    }
  },
  mounted () {
    this.time = this.value
  }
}
</script>

<style scoped>

</style>

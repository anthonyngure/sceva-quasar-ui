<template>
  <q-input outlined dense v-model="date" mask="####-##-##" :label="label || 'Date'" :disable="loading">
    <template v-slot:append>
      <q-icon name="event" class="cursor-pointer">
        <q-popup-proxy transition-show="scale" transition-hide="scale">
          <q-date v-model="date" mask="YYYY-MM-DD">
            <div class="row items-center justify-end q-gutter-sm">
              <q-btn label="Cancel" color="primary" flat v-close-popup/>
              <q-btn label="OK" color="primary" flat v-close-popup/>
            </div>
          </q-date>
        </q-popup-proxy>
      </q-icon>
    </template>
  </q-input>
</template>

<script>
export default {
  name: 'ScevaDateInput',
  props: {
    value: { required: false },
    label: {
      type: String,
      required: false
    },
    loading: {
      type: Boolean,
      required: false,
      default: false
    }
  },
  data () {
    return {
      date: null
    }
  },
  watch: {
    value (val) {
      this.$emit('input', (val || '').replace(' 00:00:00', ''))
    },
    date (val) {
      this.$emit('input', (val || '').replace(' 00:00:00', ''))
    }
  },
  mounted () {
    setTimeout(() => {
      this.date = (this.value || '').replace(' 00:00:00', '')
    }, 500)
  }
}
</script>

<style scoped>

</style>

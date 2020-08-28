<template>
  <q-form autocorrect="off" autocapitalize="off" autocomplete="off" spellcheck="false"
          @submit="$emit('submit')">
    <q-card :flat="flat" :bordered="bordered">

      <slot name="top"/>

      <q-card-section>
        <div class="row q-col-gutter-md">
          <slot/>
        </div>
      </q-card-section>

      <sceva-progress-card-section v-if="loading"/>

      <q-card-actions align="right">
        <slot name="before-continue-btn"></slot>
        <q-btn type="submit" :class="fullWidthButton ? 'full-width' : ''" label="Continue" color="primary" unelevated
               :disable="loading"/>
        <slot name="after-continue-btn"></slot>
      </q-card-actions>

      <q-card-section>
        <slot name="bottom"/>
      </q-card-section>

    </q-card>
  </q-form>
</template>

<script>
import ScevaProgressCardSection from './ScevaProgressCardSection'

export default {
  name: 'ScevaForm',
  components: { ScevaProgressCardSection },
  props: {
    loading: {
      type: Boolean,
      required: false,
      default: false
    },
    fullWidthButton: {
      type: Boolean,
      required: false,
      default: false
    },
    bordered: {
      type: Boolean,
      required: false,
      default: false
    },
    flat: {
      type: Boolean,
      required: false,
      default: false
    }
  }
}
</script>

<style scoped>

</style>

<template>
  <q-card flat class="text-center full-height bg-primary text-white">
    <q-item clickable>
      <q-item-section side>
        <q-icon :name="item.icon" color="white"/>
      </q-item-section>
      <q-item-section>
        <q-item-label lines="1" class="text-subtitle2 font-size-10 text-uppercase text-white">{{item.label}}</q-item-label>
      </q-item-section>
    </q-item>
    <q-separator dark/>
    <q-card-section>
      <div class="font-size-2 text-bold">
        {{item.value}}
      </div>
    </q-card-section>
  </q-card>
</template>

<script>
export default {
  name: 'ScevaSummaryCard',
  props: {
    item: {
      type: Object,
      required: true
    }
  }
}
</script>

<style scoped>

</style>

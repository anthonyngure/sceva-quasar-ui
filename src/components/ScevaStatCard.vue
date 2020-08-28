<template>
  <q-card :class="`full-height`">
    <q-card-section>
      <div class="text-h6">{{card.value}} {{card.title || '.'}}</div>
    </q-card-section>
    <q-card-section v-if="parseInt(card.data.join('')) !== 0">
      <trend :data="card.data"
             :gradient="['#6fa8dc', '#42b983', '#2c3e50']"
             auto-draw smooth/>
    </q-card-section>
    <q-card-section v-else>
      <q-separator color="red" spaced/>
    </q-card-section>
  </q-card>
</template>

<script>
export default {
  name: 'ScevaStatCard',
  props: {
    card: {
      type: Object,
      required: true
    }
  }
}
</script>

<style scoped>

</style>

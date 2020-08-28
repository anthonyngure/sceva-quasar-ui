<template>
  <q-item clickable replace :key="item.title" :to="{name: item.route, params: item.params}" >
    <q-item-section avatar v-if="!noIcon">
      <q-icon :name="item.icon"/>
    </q-item-section>
    <q-item-section>
      <q-item-label>{{ item.title }}</q-item-label>
    </q-item-section>
  </q-item>
</template>

<script>
export default {
  name: 'ScevaDrawerItem',
  props: {
    item: {
      type: Object,
      required: true
    },
    noIcon: {
      type: Boolean,
      required: false,
      default: false
    }
  }
}
</script>

<style scoped>

</style>

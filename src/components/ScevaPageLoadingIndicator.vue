<template>
  <q-card flat>
    <q-item>
      <q-item-section avatar>
        <q-skeleton type="QAvatar"/>
      </q-item-section>
      <q-item-section>
        <q-item-label>
          <q-skeleton type="rect"/>
        </q-item-label>
        <q-item-label caption>
          <q-skeleton type="rect"/>
        </q-item-label>
      </q-item-section>
    </q-item>

    <q-skeleton height="200px" square/>

    <q-card-actions align="right" class="q-gutter-md">
      <q-skeleton type="QBtn"/>
      <q-skeleton type="QBtn"/>
    </q-card-actions>
  </q-card>
</template>

<script>
export default {
  name: 'ScevaPageLoadingIndicator'
}
</script>

<style scoped>

</style>

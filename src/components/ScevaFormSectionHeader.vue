<template>
  <div class="q-px-md q-pb-none q-mb-none text-overline text-bold col-xs-12">{{title}}</div>
</template>

<script>
export default {
  name: 'ScevaFormSectionHeader',
  props: {
    title: {
      type: String,
      required: false
    }
  }
}
</script>

<style scoped>

</style>

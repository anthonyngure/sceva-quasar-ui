<template>
  <div class="row justify-between">
    <div class="row">
      <q-btn color="primary" :label="backText || 'Back'" icon="arrow_back" @click="goBack" flat :no-caps="$q.screen.lt.sm"
             :dense="$q.screen.lt.sm"/>
      <q-btn icon="refresh" flat @click="$emit('refresh')" :disable="loading" v-if="withRefresh"
             :dense="$q.screen.lt.sm" color="primary"/>
      <div v-if="title" :class="`text-h6 q-ml-md text-uppercase`" v-line-clamp:20="1">
        {{title}}
        <slot name="title"></slot>
      </div>
    </div>
    <div>
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ScevaBackButton',
  props: {
    routeName: {
      type: String,
      required: false
    },
    title: {
      type: String,
      required: false
    },
    backText: {
      type: String,
      required: false
    },
    withRefresh: {
      type: Boolean,
      required: false
    },
    loading: {
      type: Boolean,
      required: false
    }
  },
  methods: {
    goBack () {
      if (this.routeName) {
        this.$router.push({ name: this.routeName })
      } else {
        this.$router.go(-1)
      }
    }
  }
}
</script>

<style scoped>

</style>

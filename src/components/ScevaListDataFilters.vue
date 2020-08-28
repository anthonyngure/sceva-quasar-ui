<template>
  <q-card>
    <q-expansion-item
      expand-icon-toggle>

      <template v-slot:header>
        <q-item-section>
          Filters
        </q-item-section>

        <q-item-section side>
          <div class="row items-center">
            <q-btn icon="refresh" round flat size="sm" @click="$emit('refresh')"/>
          </div>
        </q-item-section>
      </template>

      <q-card>
        <q-card-section class="q-pt-none">
          <div class="row q-col-gutter-sm">
            <div class="col" v-for="(filter,index) in filters" :key="`${index}-${filter.label}`">
              <div class="text-body2">{{filter.label}}</div>
              <q-select outlined dense v-model="filter.value" :options="filter.options || []"
                        :aria-placeholder="filter.label" :placeholder="filter.label" :disable="loading"
                        transition-show="jump-down" transition-hide="jump-up"/>
            </div>
          </div>
        </q-card-section>
      </q-card>
    </q-expansion-item>
  </q-card>
</template>

<script>
export default {
  name: 'ScevaListDataFilters',
  props: {
    filters: {
      required: true,
      type: Array
    },
    loading: {
      required: false,
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      addressType: null
    }
  }
}
</script>

<style scoped>

</style>

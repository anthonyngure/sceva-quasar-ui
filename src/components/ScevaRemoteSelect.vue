<template>
  <q-select ref="qSelect" dense outlined v-model="option" :loading="loading || fetchingSelectedSearchOption"
            :disable="disable || fetchingSelectedSearchOption"
            :label="label || 'Search'" :options="transformedSearchResults"
            option-value="id" option-label="name" use-chips emit-value map-options
            @filter="(val, update, abort) => search({val, update, abort})"
            use-input input-debounce="1000" hide-dropdown-icon lazy-rules
            :rules="[ val => !!val || `${label} is required`]"
            :multiple="multiple" new-value-mode="add-unique" hide-bottom-space/>
</template>

<script>
import { createNamespacedHelpers } from 'vuex'

export default {
  name: 'ScevaRemoteSelect',
  props: {
    value: {
      required: false
    },
    multiple: {
      type: Boolean,
      required: false
    },
    disable: {
      type: Boolean,
      required: false
    },
    label: {
      type: String,
      required: false
    },
    store: {
      type: String,
      required: true
    }
  },
  data () {
    return {
      option: null,
      loadedSearchOption: null
    }
  },
  watch: {
    option (val) {
      this.$emit('input', val)
    },
    selectedSearchOption (val) {
      this.loadedSearchOption = val
      this.option = val.id
    }
  },
  beforeCreate () {
    const namespace = this.$options.propsData.store
    const { mapState, mapActions } = createNamespacedHelpers(namespace)
    this.$options.computed = {
      ...mapState({
        searchResults: state => state.searchResults,
        selectedSearchOption: state => state.selectedSearchOption,
        fetchingSelectedSearchOption: state => state.fetchingSelectedSearchOption,
        loading: state => state.searching
      }),
      transformedSearchResults () {
        const options = this.searchResults || []
        const updatedOptions = [...options]
        if (this.selectedSearchOption) {
          updatedOptions.push(this.selectedSearchOption)
        }
        if (this.loadedSearchOption) {
          updatedOptions.push(this.loadedSearchOption)
        }
        return [...new Set(updatedOptions)]
      }
    }
    this.$options.methods = {
      ...mapActions(['search', 'fetchSelectedSearchOption'])
    }
  },
  mounted () {
    setTimeout(() => {
      if (this.value instanceof Object) {
        this.option = this.value.id
        this.fetchSelectedSearchOption(this.value.id)
      } else if (this.value) {
        this.option = this.value
        this.fetchSelectedSearchOption(this.value)
      }
    }, 500)
  }
}
</script>

<style scoped>

</style>

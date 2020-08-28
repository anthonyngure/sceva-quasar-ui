<template>
  <q-page padding>

    <q-card>

      <q-card-section>
        <back-button :title="`${$app.singularName(store)} details`" @refresh="fetchSelected($route.params.id)"
                     with-refresh/>
      </q-card-section>

      <q-separator/>

      <template v-if="item && !loadingSelected && !deletingSelected">

        <q-card-section>
          <slot name="top" v-bind:item="item"/>
        </q-card-section>

        <q-card-section>
          <sceva-item-data :item="item" :dense="dense" :hidden="hidden" :long-texts="longTexts"/>
        </q-card-section>

        <q-card-section>
          <slot name="bottom" v-bind:item="item"/>
        </q-card-section>

        <!--Actions-->
        <q-card-actions align="right">
          <q-btn v-if="editRoute" color="primary" label="Edit" icon="edit" outline
                 :to="{name: editRoute, params: {id: item.id}}"/>
          <q-btn v-if="deletable" color="red" label="Delete" icon="delete" outline
                 @click="promptDelete()" :disable="loadingSelected||deletingSelected"/>
        </q-card-actions>

      </template>

      <!--Loading indicators-->
      <q-card-section v-else-if="loadingSelected || deletingSelected">
        <page-loading-indicator/>
      </q-card-section>

    </q-card>

  </q-page>
</template>

<script>
import { createNamespacedHelpers } from 'vuex'
import PageLoadingIndicator from '../../components/PageLoadingIndicator'
import ScevaItemData from './ScevaItemData'
import BackButton from '../../components/BackButton'

export default {
  name: 'ScevaItemDetailsPage',
  components: {
    BackButton,
    ScevaItemData,
    PageLoadingIndicator
  },
  props: {
    editRoute: {
      type: String,
      required: false
    },
    store: {
      type: String,
      required: true
    },
    dense: {
      type: Boolean,
      required: false,
      default: false
    },
    deletable: {
      type: Boolean,
      required: false,
      default: false
    },
    hidden: {
      type: Array,
      required: false,
      default: function () {
        return []
      }
    },
    longTexts: {
      type: Array,
      required: false,
      default: function () {
        return []
      }
    }
  },
  computed: {},
  beforeCreate () {
    const namespace = this.$options.propsData.store
    const { mapState, mapActions } = createNamespacedHelpers(namespace)
    this.$options.computed = {
      ...mapState({
        item: state => state.selected,
        loadingSelected: state => state.loadingSelected,
        deletingSelected: state => state.deletingSelected
      }),
      hasDefaultSlot () {
        return !!this.$slots.default
      }
    }
    this.$options.methods = {
      ...mapActions(['fetchSelected', 'deleteSelected']),
      promptDelete () {
        const self = this
        this.$q.dialog({
          title: 'Confirm',
          message: 'Are you sure you would like to delete this item ?',
          cancel: true,
          persistent: true
        }).onOk(() => {
          // console.log('>>>> OK')
          self.deleteSelected(self.$route.params.id)
        })
      }
    }
  },
  created () {
    this.fetchSelected(this.$route.params.id)
  }
}
</script>

<style scoped>

</style>

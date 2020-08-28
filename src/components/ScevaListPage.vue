<template>
  <q-page>
    <q-card flat>

      <!--Stats-->
      <q-card-section v-if="statCards.length">
        <div class="row q-col-gutter-sm">
          <div class="col-xs-12 col-sm-6 col-md-3" v-for="(card,index) in statCards" :key="index">
            <stat-card :card="card"/>
          </div>
        </div>
      </q-card-section>

      <!--Filters-->
      <q-card-section v-if="filters.length">
        <list-data-filters :filters="filters" :loading="loading" @refresh="refresh(pagination, filter)"/>
      </q-card-section>

      <!--Data-->
      <q-card-section>
        <q-table :title="$app.camelCaseToWords(store).toUpperCase()" :data="sortedData" :columns="columns" row-key="id"
                 :filter="filter" :loading="loading" :pagination.sync="pagination" @request="onRequest">
          <!--Top slot-->
          <template v-slot:top v-if="addRoute">
            <q-btn :label="`Create a new ${$app.singularName(store)}`" color="primary" flat icon="add"
                   :to="{name: addRoute}" :class="$q.screen.xs ? 'full-width' : ''"/>
            <q-space/>
            <q-input dense debounce="300" v-model="filter" placeholder="Search"
                     :class="$q.screen.xs ? 'full-width' : ''">
              <template v-slot:append>
                <q-icon name="search"/>
                <q-btn icon="refresh" v-if="!filters.length" dense round flat @click="refresh(pagination, filter)"/>
              </template>
            </q-input>
          </template>
          <template v-slot:top-right v-else>
            <q-space/>
            <q-input dense debounce="300" v-model="filter" placeholder="Search"
                     :class="$q.screen.xs ? 'full-width' : ''">
              <template v-slot:append>
                <q-icon name="search"/>
                <q-btn icon="refresh" v-if="!filters.length" dense round flat @click="refresh(pagination, filter)"/>
              </template>
            </q-input>
          </template>
          <template v-slot:body-cell="props">
            <td :props="props"
                :class="`${props.col.longText ? 'long-text-cell' : ''} text-${props.col.align}`"
                v-if="props.col.name !== 'actions'">
              {{props.value}}
            </td>
            <q-td :props="props" v-else>
              <q-btn v-if="editRoute" flat round icon="edit" color="green" dense
                     :to="{name: editRoute, params: {id: props.row.id}}"/>
              <q-btn v-if="detailsRoute" flat round icon="open_in_new" color="primary" dense
                     :to="{name: detailsRoute, params: {id: props.row.id}}"/>
            </q-td>
          </template>
          <slot name="cells"/>
        </q-table>

      </q-card-section>

    </q-card>

  </q-page>
</template>

<script>
import { createNamespacedHelpers } from 'vuex'
import StatCard from './ScevaStatCard'
import ListDataFilters from './ScevaListDataFilters'

export default {
  name: 'ScevaListPage',
  components: {
    ListDataFilters,
    StatCard
  },
  props: {
    addRoute: {
      type: String,
      required: false
    },
    editRoute: {
      type: String,
      required: false
    },
    detailsRoute: {
      type: String,
      required: false
    },
    columns: {
      type: Array,
      required: true
    },
    filters: {
      type: Array,
      required: false,
      default: function () {
        return []
      }
    },
    store: {
      type: String,
      required: true
    }
  },
  data () {
    return {
      filter: null,
      pagination: {
        sortBy: 'id',
        descending: true,
        page: 1,
        rowsPerPage: 10,
        rowsNumber: 0
      }
    }
  },
  beforeCreate () {
    const namespace = this.$options.propsData.store
    const { mapState, mapActions } = createNamespacedHelpers(namespace)
    this.$options.computed = {
      ...mapState({
        data: state => state.data,
        loading: state => state.loading
      }),
      sortedData () {
        if (this.data && this.data.data) {
          return [...this.data.data].sort((a, b) => b.id - a.id)
        }
        return []
      },
      statCards () {
        if (this.data && this.data.statCards && this.data.statCards.length) {
          return this.data.statCards
        }
        return []
      }
    }
    this.$options.methods = {
      ...mapActions(['fetch']),
      refresh (pagination, filter) {
        this.$app.log(pagination)
        this.$app.log(filter)
        const self = this
        this.fetch({
          pagination: pagination,
          filter: filter,
          refresh: false,
          callback: {
            onSuccess (data) {
              // alert(data)
              self.pagination.rowsNumber = data.total || 0
              self.pagination.page = data.currentPage || 1
              self.pagination.rowsPerPage = data.perPage || 10
              self.pagination.sortBy = data.sortBy || 'id'
              self.pagination.descending = data.sortDirection === 'desc'
            }
          }
        })
      },
      onRequest (props) {
        this.refresh(props.pagination, props.filter)
      }
    }
  },
  created () {
    this.refresh(this.pagination, this.filter)
  }
}
</script>

<style scoped>

</style>

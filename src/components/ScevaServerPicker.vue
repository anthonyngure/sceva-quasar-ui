<template>
  <q-btn-dropdown flat dense :label="server? server.name : 'Select server'">
    <q-list>
      <q-item v-for="server in servers" :key="server.url" clickable v-close-popup @click="switchServer(server)">
        <q-item-section>
          <q-item-label>{{ server.name }}</q-item-label>
        </q-item-section>
      </q-item>
    </q-list>
  </q-btn-dropdown>
</template>

<script>
export default {
  name: 'ScevaServerPicker',
  data () {
    return {
      server: null,
      servers: [
        {
          name: 'STAGING',
          appKey: 'FV1lo4Z8eB21GgKN',
          url: 'http://54.85.77.94:4066/v1'
        },
        {
          name: 'LIVE',
          appKey: 'FV1lo4Z8eB21GgKN',
          url: 'https://api.sceva.co.ke/v1'
        },
        {
          name: 'LOCAL',
          appKey: 'FV1lo4Z8eB21GgKN',
          url: 'http://192.168.0.117:8000/v1'
        }
      ]
    }
  },
  methods: {
    switchServer (server) {
      this.$q.localStorage.set('server', server)
      this.$router.push({ name: 'login' })
    }
  },
  mounted () {
    const lastSelectedServer = this.$q.localStorage.getItem('server')
    this.server = lastSelectedServer || this.servers[0]
  }
}
</script>

<style scoped>

</style>

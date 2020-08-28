<template>
  <q-markup-table flat :dense="dense">
    <tbody>
    <tr v-for="dataRow in visibleDataRows" :key="dataRow.label">
      <td class="text-left">{{dataRow.label}}</td>
      <td :class="dataRow.longText ? 'text-left long-text-cell' : 'text-left'" style="width: 80%">
        <span v-html="dataRow.value"></span>
      </td>
    </tr>
    </tbody>
  </q-markup-table>
</template>

<script>
export default {
  name: 'ScevaItemData',
  props: {
    item: {
      type: Object,
      required: true
    },
    dense: {
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
  computed: {
    visibleDataRows () {
      if (!this.item) {
        return []
      } else {
        const keys = Object.keys(this.item)
        const dataRows = []
        const that = this
        keys.forEach(function (key) {
          if (that.showKey(key)) {
            dataRows.push({
              label: that.$app.attributeToWords(key),
              value: that.readValue(key),
              longText: that.isLongText(key)
            })
          }
        })
        return dataRows
      }
    }
  },
  methods: {
    showKey (key) {
      return this.hidden.indexOf(key) === -1
    },
    isLongText (key) {
      return this.longTexts.indexOf(key) >= 0
    },
    readValue (key) {
      // In most cases the nested items have name
      const value = this.item[key]
      if (this.isArrayOfStrings(value)) {
        return value.join(',')
      } else if (value instanceof Array && key === 'phones') {
        return value.map(function (phone) {
          return phone.number
        }).join(', ')
      } else if (Array.isArray(value)) {
        return value.length > 0 ? value.length : 'N/A'
      } else if (value instanceof Object && (key === 'user' || key === 'referred')) {
        return value.loginId || value.name || value
      } else if (value instanceof Object) {
        return value.name || value.number || value
      } else {
        return value || 'N/A'
      }
    },
    isArrayOfStrings (value) {
      if (!Array.isArray(value)) {
        return false
      }
      const firstObject = value[0]
      return this.$app.isString(firstObject)
    }
  }
}
</script>

<style scoped>

</style>

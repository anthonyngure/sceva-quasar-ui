<template>
  <q-card flat bordered>
    <q-card-section>
      <div class="text-body2">{{hint || 'Select images'}}</div>
    </q-card-section>

    <progress-card-section v-if="deletingMedia"/>

    <q-separator/>

    <!--Images-->
    <q-card-section v-if="tableData && tableData.length">
      <div class="row q-col-gutter-sm">
        <div class="col-xs-12 col-sm-4 col-md-3" v-for="(item, index) in tableData" :key="index">
          <q-card bordered flat style="width: 100%">
            <q-card-section style="height: 120px" class="bg-grey-3">
              <!--suppress HtmlUnknownTarget -->
              <img v-if="item.type === 'file'" :src="item.item.blob" alt="Image" class="responsive-image"/>
              <!--suppress HtmlUnknownTarget -->
              <img v-else :src="item.item.url" alt="Image" class="responsive-image"/>
            </q-card-section>
            <q-card-actions align="right">
              <q-btn :label="`Size: ${parseInt(item.item.size/1024)} KB`" flat dense size="sm"/>
              <q-btn dense flat icon="delete" round color="red" :disable="deletingMedia" size="sm"
                     @click="item.type === 'media' ? deleteMedia(item.item) : $refs.upload.remove(item.item)"/>
            </q-card-actions>
          </q-card>
        </div>
      </div>
    </q-card-section>
    <q-separator/>
    <q-card-section>
      <file-upload ref="upload" :draggable="true" :drop-directory="false" :drop="true"
                   v-if="!deletingMedia" class="full-width"
                   v-model="files" @input-filter="inputFilter" :multiple="true" :disabled="deletingMedia"
                   extensions="jpg,gif,png,webp" accept="image/*" :size="1024 * 1024" :maximum="8">
        <q-card bordered flat square class="text-center text-caption q-pa-md">
          Click to select or drag and drop files here
        </q-card>
      </file-upload>
    </q-card-section>

  </q-card>

</template>

<script>
import ProgressCardSection from '../../components/ProgressCardSection'

export default {
  name: 'ScevaMediaPicker',
  components: { ProgressCardSection },
  props: {
    hint: {
      type: String,
      required: false
    },
    media: {
      type: Array,
      required: false,
      default: function () {
        return []
      }
    }
  },
  data () {
    return {
      files: [],
      deletingMedia: false
    }
  },
  watch: {
    files (val) {
      this.$emit('input', val)
    }
  },
  computed: {
    currentExisting () {
      return this.existing
    },
    tableData () {
      const data = []
      if (this.media) {
        this.media.forEach(function (media) {
          data.push({
            type: 'media',
            item: media
          })
        })
      }
      if (this.files) {
        this.files.forEach(function (imageObject) {
          data.push({
            type: 'file',
            item: imageObject
          })
        })
      }
      return data
    }
  },
  methods: {
    clear () {
      this.files = []
    },
    inputFilter (newFile, oldFile, prevent) {
      if (newFile && !oldFile) {
        // Add file

        // Filter non-image file
        // Will not be added to files
        if (!/\.(jpeg|jpe|jpg|gif|png|webp)$/i.test(newFile.name)) {
          return prevent()
        }
        // Create the 'blob' field for thumbnail preview
        newFile.blob = ''
        const URL = window.URL || window.webkitURL
        if (URL && URL.createObjectURL) {
          newFile.blob = URL.createObjectURL(newFile.file)
        }
      }

      if (newFile && oldFile) {
        // Update file

        // Increase the version number
        if (!newFile.version) {
          newFile.version = 0
        }
        newFile.version++
      }

      if (!newFile && oldFile) {
        // Remove file

        // Refused to remove the file
        // return prevent()
      }
    },
    deleteMedia (media) {
      const self = this
      this.$client.delete(`admin/media/${media.id}`, {
        onSuccess (data) {
          self.$emit('mediaDeleted', data)
        },
        onConnectionStatusChange (status) {
          self.deletingMedia = status
        }
      })
    }
  }
}
</script>

<style scoped>

</style>

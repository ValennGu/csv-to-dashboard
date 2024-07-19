<script setup>
import { ref } from 'vue'

const props = defineProps({
  maxSize: Number,
  extension: String
})
const emit = defineEmits(['data'])

const error = ref(null)
const file = ref({
  name: '',
  size: 0,
  extension: '',
})

const handleFileChange = (e) => {
  error.value = null

  if (e.target.files && e.target.files[0]) {
    const upload = e.target.files[0];

    file.value.name = upload.name.split('.').shift()
    file.value.extension = upload.name.split('.').pop()
    file.value.size = upload.size

    if (isFileExtensionValid(file.value.extension) && isFileSizeValid(file.value.size)) {
      const reader = new FileReader()
      reader.onload = (evt) => {
        emit('data', evt.target.result)
      }

      reader.readAsText(upload)
    }
  }
}

const isFileSizeValid = (size) => {
  if (size > props.maxSize) {
    error.value = 'File is too large.'
    return false
  }

  return true
}

const isFileExtensionValid = (ext) => {
  if (ext !== props.extension) {
    error.value = `Extension ${ext} is not supported file must be in ${props.extension} format.`
    return false
  }

  return true
}
</script>

<template>
  <input type="file" name="" id="" @change="handleFileChange($event)">
  <div class="info" v-if="!error.value">
    ----
    <span>File name: {{ file.name || "--" }}</span>
    <span>File size: {{ file.size || "--" }}</span>
    <span>File extension: {{ file.extension || "--" }}</span>
  </div>

  <div v-if="error">
    {{ error }}
  </div>
</template>

<style>
  .info {
     display: flex;
     flex-wrap: wrap;
  }

  span {
    width: 100%;
  }
</style>
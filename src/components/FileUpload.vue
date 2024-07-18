<script setup>
import { ref } from 'vue'

const file = ref({
  name: '',
  size: 0,
  extension: '',
  isUploaded: false,
})

const error = ref(null)

const handleFileChange = (e) => {
  if (e.target.files && e.target.files[0]) {
    const upload = e.target.files[0];

    if (isFileSizeValid(upload.size)) {
      file.value.name = upload.name.split('.').shift()
      file.value.extension = upload.name.split('.').pop()
      file.value.size = upload.size
      file.value.isUploaded = true

      const reader = new FileReader()
      
      reader.onloadend = (e) => {
        console.log('Done loading')
      }
    }
  }
}

const isFileSizeValid = (size) => {
  if (size > 50) {
    error.value = 'File is too large'
    return false
  }

  error.value = null
  return true
}
</script>

<template>
  <input type="file" name="" id="" @change="handleFileChange($event)">
  <div class="info" v-if="!error">
    ----
    <span>File name: {{ file.name || "No file selected" }}</span>
    <span>File size: {{ file.size || "No file selected" }}</span>
    <span>File extension: {{ file.extension || "No file selected" }}</span>
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
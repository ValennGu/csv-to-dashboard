<script setup lang="ts">
import { ref } from 'vue'
import FileUpload from '@/components/FileUpload.vue'

const data = ref('')

const handleDataChange = (data) => {
  data.value = data
}
</script>

<template>
  <FileUpload :maxSize="500000" extension="csv" @data="(data) => handleDataChange(data)" />
  {{ data }}
</template>

<style></style>

<script setup>
import FileUpload from '@/components/FileUpload.vue'

const handleDataChange = (data) => {
  console.log(data)
}
</script>

<template>
  <FileUpload :maxSize="500000" extension="csv" @data="(data) => handleDataChange(data)"/>
</template>

<style></style>

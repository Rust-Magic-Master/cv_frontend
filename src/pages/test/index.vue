<script setup lang="ts">
import { ref } from 'vue'

// State management
const fileName = ref<string | null>(null)
const originalImage = ref<string | null>(null)
const processedImageUrl = ref<string | null>(null)

// File handling
function onFileChange(e: Event) {
  const file = (e.target as HTMLInputElement).files?.[0]
  if (!file)
    return

  fileName.value = file.name
  const reader = new FileReader()
  reader.onload = (e) => {
    originalImage.value = e.target?.result as string
  }
  reader.readAsDataURL(file)
}

// API interaction
async function sendImage() {
  if (!fileName.value)
    return

  try {
    const response = await fetch(`${import.meta.env.VITE_BASE_URL}/test`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ file_name: fileName.value }),
    })

    const { processed_file_name } = await response.json()
    processedImageUrl.value = `${import.meta.env.VITE_BASE_URL}/assets/${processed_file_name}`
  }
  catch (error) {
    console.error('Error processing image:', error)
  }
}
</script>

<template>
  <div>
    <div>
      <input type="file" @change="onFileChange">
      <button :disabled="!fileName" @click="sendImage">
        上传并处理
      </button>
    </div>

    <div class="mt-4 flex flex-row justify-center gap-4">
      <div v-if="fileName" class="image-box mt-4 flex flex-col items-center">
        <p>选择的文件: {{ fileName }}</p>
        <img v-if="originalImage" class="h-auto w-200px" :src="originalImage" alt="原图">
      </div>

      <div v-if="processedImageUrl" class="image-box mt-4 flex flex-col items-center">
        <p>处理后的文件</p>
        <img class="h-auto w-200px" :src="processedImageUrl" alt="处理后的图片">
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
const fileName = ref<string | null>(null)
const filePath = ref<string | null>(null)
const originalImage = ref<string | null>(null)

function onFileChange(e: Event) {
  const target = e.target as HTMLInputElement
  const file = target.files?.[0]
  if (file) {
    fileName.value = file.name
    filePath.value = `${import.meta.env.VITE_ROOT_PHOTO_PATH}/${fileName.value}`
    const reader = new FileReader()
    reader.onload = (e) => {
      if (e.target) {
        originalImage.value = e.target.result as string
      }
    }
    reader.readAsDataURL(file)
  }
}

async function sendImage() {
  if (!fileName.value)
    return

  fetch('http://localhost:3000/test', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({ file_path: filePath.value }),
  })
    .then((res) => {
      console.warn(res)
    })
}
</script>

<template>
  <div>
    <div>
      <input type="file" @change="onFileChange">
      <button :disabled="!fileName" @click="sendImage">
        上传并处理
      </button>
      <div v-if="fileName" class="mt-4 flex flex-col items-center">
        <p>选择的文件: {{ fileName }}</p>
        <img v-if="originalImage" class="h-auto w-200px" :src="originalImage" alt="原图">
      </div>
    </div>
  </div>
</template>

<script setup>

import { onMounted, ref } from 'vue'
import { Hydra } from 'alcaeus/web'

const api = ref('')
const data = ref({})
const operations = ref([])

async function update (url) {
  const { response, representation } = await Hydra.loadResource(url)
  const rootResource = representation.root
  data.value = rootResource.toJSON()
  console.log(rootResource.findOperations())
  operations.value = rootResource.findOperations().map((current) => current.toJSON())
}

onMounted(async () => {
  api.value = 'http://localhost:8080/'
  await update(api.value)
})

</script>

<template>
  <input v-model="api"/>
  <a @click="update(api)">Update</a>
  <h2>{{ api }}</h2>
  <json-viewer
      :expand-depth=30
      :value="data"
      boxed
      copyable
      sort></json-viewer>
  <h2>Operations</h2>
  <template v-for="operation in operations">
    <json-viewer
        :expand-depth=5
        :value="operation"
        boxed
        copyable
        sort></json-viewer>
  </template>

</template>

<style>

</style>

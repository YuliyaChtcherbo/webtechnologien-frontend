<script setup lang="ts">
import { ref, onMounted } from 'vue'
import axios from 'axios'
import type { AxiosResponse } from 'axios'
import type { Travel } from '@/types'
import type { Ref } from 'vue'

const items: Ref<Travel[]> = ref([])

async function loadThings() {
  const baseUrl = import.meta.env.VITE_BACKEND_BASE_URL
  const endpoint = baseUrl + '/'
  const response: AxiosResponse = await axios.get(endpoint)
  const responseData: Travel[] = response.data
  responseData.forEach((travel: Travel) => {
    items.value.push(travel)
  })
}

onMounted(async () => {
  await loadThings()
})
</script>

<template>
  <h2>Meine Reisen</h2>
  <ul>
    <li v-for="item in items" :key="item.id">
      {{ item.id }}: {{ item.ziel }}, {{ item.startDatum }} bis {{ item.endDatum }} ({{
        item.status ? 'erledigt' : 'geplant'
      }})
    </li>
  </ul>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import axios from 'axios'
import type { AxiosResponse } from 'axios'
import type { Travel } from '@/types'

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
</script>

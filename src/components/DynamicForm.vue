<script setup lang="ts">
import { ref, onMounted } from 'vue'
import axios from 'axios'
import type { Travel } from '@/types'

const baseUrl = import.meta.env.VITE_BACKEND_BASE_URL

// Liste der Reisen aus der Datenbank
const items = ref<Travel[]>([])

// Wem gehören die Reisen (Multi-User über "owner")
const owner = ref('Eda')

// Formularfelder (über v-model mit den Inputs verbunden)
const ziel = ref('')
const startDatum = ref('')
const endDatum = ref('')
const status = ref(false)

// GET: Reisen des Owners aus dem Backend laden
async function loadThings() {
  const response = await axios.get<Travel[]>(`${baseUrl}/travellings`, {
    params: { owner: owner.value },
  })
  items.value = response.data
}

// POST: neue Reise speichern, danach Liste neu laden
async function saveTravel() {
  if (!ziel.value || !startDatum.value || !endDatum.value) return

  const newTravel: Travel = {
    ziel: ziel.value,
    startDatum: startDatum.value,
    endDatum: endDatum.value,
    status: status.value,
    owner: owner.value,
  }
  await axios.post(`${baseUrl}/travellings`, newTravel)

  ziel.value = ''
  startDatum.value = ''
  endDatum.value = ''
  status.value = false

  await loadThings()
}

onMounted(loadThings)
</script>

<template>
  <h2>Meine Reisen</h2>

  <label>
    Name:
    <input v-model="owner" @change="loadThings" />
  </label>

  <form @submit.prevent="saveTravel">
    <input v-model="ziel" placeholder="Ziel" required />
    <input v-model="startDatum" placeholder="Start (TT.MM.JJJJ)" required />
    <input v-model="endDatum" placeholder="Ende (TT.MM.JJJJ)" required />
    <label>
      <input type="checkbox" v-model="status" />
      erledigt
    </label>
    <button type="submit">Hinzufügen</button>
  </form>

  <ul>
    <li v-for="item in items" :key="item.id">
      {{ item.ziel }}, {{ item.startDatum }} bis {{ item.endDatum }} ({{
        item.status ? 'erledigt' : 'geplant'
      }})
    </li>
  </ul>
  <p v-if="items.length === 0">Noch keine Reisen gespeichert.</p>
</template>

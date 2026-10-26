<script setup lang="ts">
interface Travel {
  ziel: string
  startDatum: string
  endDatum: string
  status: boolean
}

const travellings: Travel[] = [
  { ziel: "Berlin", startDatum:"01.10.2026", endDatum:"15.10.2026", status:true },
  { ziel: "Paris", startDatum:"22.10.2026", endDatum:"25.10.2026", status:false },
]
</script>

<template>
  <h2>Meine Reisen</h2>
  <ul>
    <li v-for="travelling in travellings" :key="travelling.ziel">
      {{ travelling.ziel }}: {{ travelling.startDatum }} bis {{ travelling.endDatum }}
    </li>
  </ul>
</template>

<script setup lang="ts">
import { store } from '~/store'
</script>

<template>
  <div>
    <div class="h-14 flex flex-row items-center px-4 bg-light-400 dark:bg-dark-900 space-x-2 text-lg">
      <grommet-icons-install />
      <span>
        Installed Packages
      </span>
      <div class="bg-green-500 w-6 h-6 text-green-900 text-sm font-bold rounded flex place-items-center place-content-center">
        {{ store.packages.length }}
      </div>
    </div>
    <div>
      <InstalledItem v-for="pack in store.packages" :key="pack.url" :name="pack.name" :description="pack.description" />
    </div>
  </div>
</template>

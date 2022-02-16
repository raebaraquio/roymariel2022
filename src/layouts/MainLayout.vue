
<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'MainLayout'
})
</script>

<template>
  <q-layout view="lHh Lpr lFf" class="bg-main">
        <q-page-container class="text-center full-width full-height">
            <router-view />
        </q-page-container>
    </q-layout>
</template>

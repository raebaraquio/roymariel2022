<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'Playlist'
})
</script>

<template>
    <div class="bg-toolbar q-py-lg q-px-lg q-pb-xl"
        id="playlist">
        <p class="text-white">
            We know you've waited almost as long as we have for this special moment.
            And we really want it to be extra special by asking you to join us on dance floor too!
        </p>
        <div class="q-pt-md">
            <div class="text-white text-uppercase q-mb-xs"
            style="letter-spacing:1px;">Song/s that will get you on your feet</div>
            <q-input filled dense class="bg-white rounded-borders">
                <template v-slot:append>
                    <q-btn flat label="Submit" class="no-capitalize"/>
                </template>
            </q-input>
        </div>
        <div class="q-pt-lg">
            <div class="text-white text-uppercase q-mb-xs"
            style="letter-spacing:1px;">Artists/Singers that'll keep you singing and dancing all night</div>
            <q-input filled dense class="bg-white rounded-borders">
                <template v-slot:append>
                    <q-btn flat label="Submit" class="no-capitalize"/>
                </template>
            </q-input>
        </div>
    </div>
</template>

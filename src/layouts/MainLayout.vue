
<script>

import Attire from 'src/components/Attire.vue'
import Closing from 'src/components/Closing.vue'
import Gift from 'src/components/Gift.vue'
import Playlist from 'src/components/Playlist.vue'
import Hashtag from 'src/components/Hashtag.vue'
import Venue from 'src/components/Venue.vue'
import Rsvp from 'src/components/Rsvp.vue'

import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'MainLayout',
  components: {
    Attire,
    Closing,
    Gift,
    Hashtag,
    Playlist,
    Rsvp,
    Venue
  },
  setup () {
    const openRsvp = ref(false)
    const toggleRsvp = () => {
      openRsvp.value = !openRsvp.value
    }
    return {
      openRsvp,
      toggleRsvp
    }
  }
})
</script>

<template>
  <q-layout view="lHh Lpr lFf" class="bg-main">
        <q-page-container class="text-center full-width full-height">
            <div class="bg-lblue q-py-xl q-px-md text-dbg"
                id="welcome">
                <span class="text-uppercase" style="letter-spacing:1px">The long wait is over.</span>
                <p class="text-bold text-h5">This is it — <br />We're finally getting married </p>
                <div class="text=center">
                    <q-card flat square
                        class="transparent q-ma-auto text-center"
                        style="margin:auto;">
                        <q-card-section>
                            <q-img src="Home_HEADER.png"
                            style="max-width:320px"/>
                        </q-card-section>
                    </q-card>
                </div>
            </div>
            <Venue></Venue>
            <Attire></Attire>
            <Gift></Gift>
            <Playlist></Playlist>
            <Hashtag></Hashtag>
            <Closing></Closing>
        </q-page-container>
        <q-footer elevated>
            <q-toolbar class="bg-toolbar q-py-sm">
                <q-img src="IMG_5193.png"
                    style="max-width:45px;fill:#415a70"/>
                <q-toolbar-title
                    class="
                        text-white text-uppercase
                        vertical-middle">
                    April 1, 2022
                </q-toolbar-title>
                <q-space class="gt-sm"/>
                <q-btn
                    color="secondary"
                    label="RSVP"
                    class="text-capitalize"
                    style="letter-spacing:1px"
                    @click="toggleRsvp"/>
            </q-toolbar>
        </q-footer>

        <q-dialog
            v-model="openRsvp"
            persistent>
            <Rsvp
                v-on:close="toggleRsvp" />
        </q-dialog>

    </q-layout>
</template>

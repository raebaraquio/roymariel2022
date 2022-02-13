
<script>

import Attire from 'src/components/Attire.vue'
import Closing from 'src/components/Closing.vue'
import Gift from 'src/components/Gift.vue'
import Playlist from 'src/components/Playlist.vue'
import Hashtag from 'src/components/Hashtag.vue'
import Venue from 'src/components/Venue.vue'
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'MainLayout',
  components: {
    Attire,
    Closing,
    Gift,
    Hashtag,
    Playlist,
    Venue
  }
})
</script>

<template>
  <q-layout view="lHh Lpr lFf" class="bg-main" style="height: 100vh">
        <q-page-container class="text-center full-height">
            <div class="bg-lblue q-py-lg q-px-md text-dbg"
                id="welcome">
                <span class="text-uppercase">The long wait is over.</span>
                <p class="text-bold">This is it — We're finally getting married </p>
                <div class="text=center">
                    <q-card flat square
                        class="bg-lcream q-ma-auto text-center"
                        style="width:45vw;height:50vh;margin:auto;text-center">
                        <q-card-section></q-card-section>
                    </q-card>
                </div>
            </div>
            <Venue></Venue>
            <Attire></Attire>
            <Gift></Gift>
            <Playlist></Playlist>
            <Hashtag></Hashtag>
            <Closing></Closing>
            <!-- <Rsvp></Rsvp> -->
        </q-page-container>
        <q-footer elevated>
            <q-toolbar class="bg-toolbar items-center">
                <q-toolbar-title
                    class="
                        text-white text-caption text-uppercase
                        vertical-middle">
                    April 1, 2022
                </q-toolbar-title>
                <q-space/>
                <div class="text-uppercase text-lbg">
                    RSVP
                </div>
            </q-toolbar>
        </q-footer>
    </q-layout>
</template>

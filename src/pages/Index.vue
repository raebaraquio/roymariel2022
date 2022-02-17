<template>
  <q-page>
    <div>
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
    </div>

    <q-footer elevated>
        <q-toolbar class="bg-toolbar rows items-center content-between">
            <div class="col text-left">
              <q-img src="RM_LOGO.png"
                  style="max-width:45px;"/>
            </div>
            <div class="col-auto text-center">
              <span class="text-white"
                style="font-size:1.25rem;letter-spacing:1px">April 1, 2022</span>
            </div>
            <div class="col text-right">
              <q-btn
                  color="secondary"
                  label="RSVP"
                  class="text-capitalize"
                  style="letter-spacing:1px"
                  @click="toggleRsvp"/>
            </div>
        </q-toolbar>
    </q-footer>

    <q-dialog
        v-model="openRsvp"
        persistent>
        <Rsvp
            v-on:close="toggleRsvp" />
    </q-dialog>
  </q-page>
</template>

<script>
import Attire from '../components/Attire.vue'
import Closing from '../components/Closing.vue'
import Gift from '../components/Gift.vue'
import Playlist from '../components/Playlist.vue'
import Hashtag from '../components/Hashtag.vue'
import Venue from '../components/Venue.vue'
import Rsvp from '../components/Rsvp.vue'

import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'PageIndex',
  components: {
    Attire,
    Closing,
    Gift,
    Hashtag,
    Playlist,
    Venue,
    Rsvp
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

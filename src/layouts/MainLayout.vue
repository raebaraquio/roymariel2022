
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

        <q-dialog class="bg-white text-dbg"
            v-model="openDialog"
            persistent >
            <q-card class="text-center q-pb-md">
                <q-card-section  class="text-dbg"
                    v-if="isCeremony">
                    <span class="text-h6 text-dbg">CEREMONY</span> <br/>
                    <span class="text-bold text-h5 text-dbg">REMINDERS</span> <br/><br/>
                    <p>
                        Save the date and don't be late! Please be there at least 45 minutes before the start of the ceremony.
                    </p>
                    <p>
                        Wearing sleeveless dress? Kindly bring a shawl or cover-up to wear when inside the church.
                    </p>
                    <p>
                        We're going unplugged! During the ceremony, please put your mobile phones aside and keep them on mute. We promise to share photos of the special moments from the ceremony!
                    </p>
                </q-card-section>
                <q-card-section  class="text-dbg"
                    v-if="isReception">
                    <span class="text-h6 text-dbg">RECEPTION</span> <br/>
                    <span class="text-bold text-h5 text-dbg">REMINDERS</span> <br/><br/>
                    <p>
                        Let's test before we toast! A rapid nasal antigen test will be conducted before entering the venue.
                    </p>
                    <p>
                        Please bring your vaccination card — just present them to our coordinators upon entry.
                    </p>
                    <p>
                        Let's keep each other safe: Practice social distancing, wash and sanitize your hands frequently, and mask up!
                    </p>
                </q-card-section>
                <q-card-action align="center">
                    <q-btn
                        flat
                        color="primary"
                        label="Close"
                        class="text-uppercase"
                        @click="closeDialog"/>
                </q-card-action>
            </q-card>
        </q-dialog>
    </q-layout>
</template>

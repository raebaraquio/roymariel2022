
<template>
    <div class="bg-toolbar q-py-lg q-px-lg q-pb-xl"
        id="playlist">
        <p class="text-white q-px-lg">
            We know you've waited almost as long as <br/>we have for this special moment. <br />
            And we really want it to be extra special <br/>by asking you to join us on the dance floor, too.
        </p>
        <div class="q-pt-md text-center">
            <div class="text-white text-uppercase q-mb-xs"
            style="letter-spacing:1px;">Song/s that will get you on your feet</div>
            <q-input
                filled dense
                bg-color="white"
                class="rounded-borders q-pr-none"
                v-model="song"
                :maxlength="50"
                :rules="[maxlength]"
                clearable
                style="max-width: 400px; margin:auto">
                <!-- @keypress.enter="addSong" -->
                <!-- <template v-slot:append>
                    <q-separator vertical inset class="q-mr-sm"/>
                    <q-btn flat dense
                      color="primary" label="Submit"
                      class="text-capitalize q-px-md"
                    @click="addSong"/>
                </template> -->
            </q-input>
        </div>
        <div class="q-pt-lg text-center">
            <div class="text-white text-uppercase q-mb-xs"
            style="letter-spacing:1px;">Artists/Singers that'll keep you singing and dancing all night</div>
            <q-input filled dense
                bg-color="white"
                class="rounded-borders"
                v-model="artist"
                :maxlength="50"
                :rules="[maxlength]"
                clearable
                @keypress.enter="addPlaylist"
                style="max-width: 400px; margin:auto">
                <!--  -->
                <!-- <template v-slot:append>
                    <q-separator vertical inset class="q-mr-sm"/>
                    <q-btn flat dense label="Submit"
                      color="primary"
                      class="text-capitalize q-px-md"
                    @click="addArtist"/>
                </template> -->
            </q-input>
        </div>
        <div class="q-mt-lg text-center">
            <q-btn
              color="secondary" label="Submit"
              class="text-capitalize q-px-xl"
              :disabled="isLoading"
              :loading="isLoading"
              @click="addPlaylist"/>
        </div>
    </div>
</template>

<script>
import { defineComponent, ref } from 'vue'
import { useClient } from '@vueauth/supabase'
import { useQuasar } from 'quasar'

export default defineComponent({
  name: 'Playlist',
  setup () {
    const $q = useQuasar()
    const supabase = useClient()
    const song = ref('')
    const artist = ref('')
    const isLoading = ref(false)

    const addPlaylist = async () => {
      const playlistData = {}
      if (song.value && song.value.trim().length > 0) {
        playlistData.song = song.value.trim()
        if (song.value.trim().length > 40) {
          $q.notify({
            message: 'Please check your input. Only a maximum of 40 characters is allowed.',
            multiLine: true,
            color: 'negative',
            timeout: 2300
          })
          return false
        }
      }
      if (artist.value && artist.value.trim().length > 0) {
        playlistData.artist = artist.value.trim()
        if (artist.value.trim().length > 40) {
          $q.notify({
            message: 'Please check your input. Only a maximum of 40 characters is allowed.',
            multiLine: true,
            color: 'negative',
            timeout: 2300
          })
          return false
        }
      }
      if (Object.keys(playlistData).length > 0) {
        isLoading.value = true
        const addResponse = await supabase
          .from('playlist')
          .insert([
            playlistData
          ])

        isLoading.value = false
        if (addResponse.error) {
          $q.notify({
            message: `Ooops! An error occurred while processing your reply. Please try again. (Error: ${addResponse.statusText})`,
            multiLine: true,
            color: 'negative',
            timeout: 2300
          })
          return false
        } else {
          $q.notify({
            message: 'Entry submitted. Thank you for answering!',
            multiLine: true,
            color: 'positive',
            timeout: 2200
          })
          // Clear field inputs
          artist.value = ''
          song.value = ''
        }
      }
    }

    const maxlength = val => val.length < 41 || 'Maximum of 40 characters only.'

    return {
      isLoading,
      maxlength,
      song,
      artist,
      addPlaylist
    }
  }
})
</script>

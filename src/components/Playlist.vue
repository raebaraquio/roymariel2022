
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
            <q-input filled dense class="bg-white rounded-borders"
                v-model="song"
                @keypress.enter="addSong">
                <template v-slot:append>
                    <q-btn flat label="Submit" class="no-capitalize"
                    @click="addSong"/>
                </template>
            </q-input>
        </div>
        <div class="q-pt-lg">
            <div class="text-white text-uppercase q-mb-xs"
            style="letter-spacing:1px;">Artists/Singers that'll keep you singing and dancing all night</div>
            <q-input filled dense class="bg-white rounded-borders"
                v-model="artist"
                @keypress.enter="addArtist">
                <template v-slot:append>
                    <q-btn flat label="Submit" class="no-capitalize"
                    @click="addArtist"/>
                </template>
            </q-input>
        </div>
    </div>
</template>

<script>
import { defineComponent, onMounted, ref } from 'vue'
// import { useClient } from '@vueauth/supabase'

export default defineComponent({
  name: 'Playlist',
  setup () {
    // const supabase = useClient()
    const song = ref('')
    const artist = ref('')

    onMounted(async () => {
    //   const { data, error } = await supabase
    //     .from('songs')
    //     .select()

    //   console.log(data)
    //   console.log(error)
    })

    const addSong = async () => {
      if (song.value && song.value.trim().length > 0) {
        // const { addResponse, addError } = await supabase
        //   .from('songs')
        //   .insert([
        //     { song: song.value.trim() }
        //   ])

        // console.log(addResponse)
        // console.log(addError)
      }
    }

    const addArtist = async () => {
      if (artist.value && artist.value.trim().length > 0) {
        // const { addResponse, addError } = await supabase
        //   .from('artists')
        //   .insert([
        //     { artist: artist.value.trim() }
        //   ])

        // console.log(addResponse)
        // console.log(addError)
      }
    }

    return {
      song,
      artist,
      addSong,
      addArtist
    }
  }
})
</script>

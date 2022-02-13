<script>
import { computed, defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'Venue',

  setup () {
    // const leftDrawerOpen = ref(false)
    const openDialog = ref(false)
    const venueOption = ref('')

    const showDialog = (mode) => {
      venueOption.value = mode
      openDialog.value = true
    }

    const closeDialog = (mode) => {
      venueOption.value = ''
      openDialog.value = false
    }

    const openLoc = (loc) => {
      window.open(loc)
    }

    const isReception = computed(() => venueOption.value === 'reception')
    const isCeremony = computed(() => venueOption.value === 'ceremony')

    return {
      isReception,
      isCeremony,
      openDialog,
      venueOption,
      showDialog,
      closeDialog,
      openLoc
    }
  }
})
</script>

<template>
    <div class="bg-lcream q-mt-md text-dbg q-py-lg q-px-md"
        id="venue">
        <p>
            We want you to be there with us as we say our "I Do's", and to join us as we celebrate our new beginning together!
        </p>
        <q-card flat square
            class="bg-lcream text-center text-dbg">
            <q-card-section>
                <!-- QR CODE -->
                <q-img
                    src="EVM_QR.png"
                    :transition="'fade'"
                    class="text-center"
                    align="center"
                    spinner-color="white"
                    @click="openLoc('https://goo.gl/maps/UYCRoAZBMfgSUTJb7')"/>
            </q-card-section>
            <q-card-section>
                <span class="text-bold">CEREMONY</span><br/>
                <div @click="openLoc('https://goo.gl/maps/UYCRoAZBMfgSUTJb7')">Iglesia ni Cristo CVM | Tagaytagy, Cavite</div>
                <h5 class="text-h5 q-my-none">2PM</h5>
                <a class="text-info text-cursor"
                @click="showDialog('ceremony')">Reminders</a>
            </q-card-section>
        </q-card>

        <q-card flat square
            class="bg-lcream text-center text-dbg">
            <q-card-section>
                <q-img
                    src="Farm-Hills_QR.png"
                    :transition="'fade'"
                    class="text-center"
                    align="center"
                    spinner-color="white"
                    @click="openLoc('https://goo.gl/maps/kd8YUP69hPB6eESN6')"/>
            </q-card-section>
            <q-card-section>
                <span class="text-bold">RECEPTION</span><br/>
                <div @click="openLoc('https://goo.gl/maps/kd8YUP69hPB6eESN6')">Farm Hills Garden | Silang, Cavite</div>
                <h5 class="text-h5 q-my-none">4PM</h5>
                <a class="text-info text-cursor"
                @click="showDialog('reception')">Reminders</a>
            </q-card-section>
        </q-card>
    </div>

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
</template>

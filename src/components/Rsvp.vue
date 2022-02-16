<template>
  <div>
    <q-card
      v-if="responseMsg === ''"
      class="text-center text-dbg"
      style="min-height:400px; max-width: 380px">
        <q-card-section  class="text-dbg">
            <span class="text-bold text-h5 text-dbg"
              style="letter-spacing: 2px">RSVP</span>
        </q-card-section>
        <q-card-section class="q-pt-xs q-pb-none">
          <div class="text-left q-mb-md">
            <span class="text-h6">*Will you be attending?</span>
            <div class="row">
              <q-radio v-model="formData.isGoing"
                color="positive"
                checked-icon="task_alt"
                unchecked-icon="panorama_fish_eye" val="Going"
                label="Yes, I'll be there." />
              <q-radio v-model="formData.isGoing"
                color="positive"
                checked-icon="task_alt"
                unchecked-icon="panorama_fish_eye" val="Not Going"
                label="Sorry, can't make it." />
            </div>
          </div>
          <div class="text-left q-my-md">
            <span class="text-h6">*Name</span>
            <div class="row items-center justify-start">
              <q-input
                filled
                dense
                label="First Name"
                class="col-12 col-2-lg col-2-md col-2-xl bg-white rounded-borders"
                :rules="[val => !!val || 'First name is required']"
                v-model="formData.firstName"
                style="max-width:350px">
              </q-input>
              <q-input
                filled
                dense
                label="Last Name"
                class="col-12 col-2-lg col-2-md col-2-xl bg-white rounded-borders"
                :rules="[val => !!val || 'Last name is required']"
                v-model="formData.lastName"
                style="max-width:350px">
              </q-input>
            </div>
          </div>
          <div class="text-left q-mt-md">
            <span class="text-h6">*Mobile Number</span>
            <div class="row items-center justify-start q-gutter-xs">
              <q-input
                filled
                dense
                placeholder="9XXXXXXXXX"
                class="col col-12-xs col-12-sm col-12-md bg-white rounded-borders"
                v-model="formData.mobile"
                :rules="[mobileFormat, required]"
                style="max-width:350px">
                <template v-slot:prepend>
                  <span class="text-caption">+63</span>
                </template>
              </q-input>
            </div>
          </div>
        </q-card-section>
        <q-card-actions align="center" class="q-pt-xs q-pb-md">
            <q-btn
                color="secondary" label="Submit"
                class="text-capitalize q-px-xl full-width"
                :disabled="isLoading"
                :loading="isLoading"
                @click="submit"
                style="max-width:360px"/>
            <q-btn
                color="grey-3" label="Close"
                unelevated
                text-color="secondary"
                class="text-capitalize q-px-xl full-width q-mt-md"
                :disabled="isLoading"
                @click="$emit('close')"
                style="max-width:360px"/>
        </q-card-actions>
    </q-card>
    <q-card v-if="responseMsg !== ''">
<q-card-section  class="text-dbg text-center">
          <span class="text-bold text-h6 text-dbg"
            v-if="responseMsg === 'going'">Hey, we're excited <br/> to celebrate with you!</span>
          <span class="text-dbg"
            v-if="responseMsg === 'sorry'">Sad to hear that <br/>you won't be able to join us.</span>
      </q-card-section>
      <q-card-section  class="text-dbg text-center q-pt-none">
          <span class="text-dbg"
            v-if="responseMsg === 'going'">Message us on Facebook <br/>and Instagram if you have questions.</span>
          <span class="text-dbg text-h6"
            v-if="responseMsg === 'sorry'">Should you wish to change your mind, you have until March 15. <br/> Otherwise, we will catch up next time!</span>
      </q-card-section>
        <q-card-actions align="center" class="q-pt-none q-pb-md">
            <q-btn
                color="grey-3" label="Close"
                unelevated
                text-color="secondary"
                class="text-capitalize q-px-xl q-px-xl q-mt-md"
                @click="$emit('close')"/>
        </q-card-actions>
    </q-card>
  </div>
</template>
<script>
import { defineComponent, ref, reactive } from 'vue'
import { useClient } from '@vueauth/supabase'
import { useQuasar } from 'quasar'

export default defineComponent({
  name: 'Rsvp',
  emits: ['close'],
  setup () {
    const $q = useQuasar()
    const supabase = useClient()
    const isLoading = ref(false)
    const responseMsg = ref('')

    const formData = reactive({
      firstName: '',
      lastName: '',
      mobile: '',
      isGoing: ''
    })

    const submit = async () => {
      responseMsg.value = ''

      if (formData.isGoing === '' ||
          formData.lastName === '' ||
          formData.firstName === '' ||
          formData.mobile === '') {
        $q.notify({
          message: 'All fields are required. Please check.',
          multiLine: true,
          color: 'negative',
          timeout: 2200
        })
        return false
      }

      isLoading.value = true

      const response = await supabase
        .from('rsvp')
        .select('mobile_number, id, first_name, last_name, is_going')
        .eq('mobile_number', `+63${formData.mobile.trim()}`)

      if (response.error) {
        return false
      }

      const data = response.data
      if (data.length > 0 && data.length === 1) {
        // Update
        const updateResponse = await supabase
          .from('rsvp')
          .update({
            first_name: formData.firstName.trim().toUpperCase(),
            last_name: formData.lastName.trim().toUpperCase(),
            is_going: formData.isGoing.trim().toUpperCase(),
            updated_at: ((new Date()).toISOString()).toLocaleString('en-US')
          })
          .eq('id', data[0].id)

        isLoading.value = false
        if (updateResponse.error) {
          return false
        }

        if (updateResponse.status === 200) {
          if (formData.isGoing === 'Going') {
            responseMsg.value = 'going'
          } else {
            responseMsg.value = 'sorry'
          }
        }
        return false
      }

      const addResponse = await supabase
        .from('rsvp')
        .insert([
          {
            first_name: formData.firstName.trim().toUpperCase(),
            last_name: formData.lastName.trim().toUpperCase(),
            is_going: formData.isGoing.trim().toUpperCase(),
            mobile_number: `+63${formData.mobile.trim()}`
          }
        ])

      isLoading.value = false
      if (addResponse.error) {
        console.log(addResponse.error)
        return
      }

      if (formData.isGoing === 'Going') {
        responseMsg.value = 'going'
      } else {
        responseMsg.value = 'sorry'
      }
    }

    const mobilePattern = /\d{10}$/gm
    const mobileFormat = val => mobilePattern.test(val) || 'Please check your mobile number.'
    const required = val => !!val || 'Mobile number is required'

    return {
      mobileFormat,
      required,
      responseMsg,
      formData,
      isLoading,
      submit
    }
  }
})
</script>

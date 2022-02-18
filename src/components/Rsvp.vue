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
                label="Sorry, I can't make it." />
            </div>
          </div>
          <div class="text-left q-my-md">
            <span class="text-h6">*Name</span>
            <div class="row items-center justify-start">
              <q-input
                filled
                dense
                clearable
                bg-color="grey-3"
                label="First Name"
                class="col-12 col-2-lg col-2-md col-2-xl rounded-borders q-mb-md"
                :rules="[val => !!val || 'First name is required', nameLength]"
                v-model="formData.firstName"
                :maxlength="50"
                style="max-width:350px">
              </q-input>
              <q-input
                filled
                dense
                clearable
                bg-color="grey-3"
                label="Last Name"
                class="col-12 col-2-lg col-2-md col-2-xl rounded-borders"
                :rules="[val => !!val || 'Last name is required', nameLength]"
                v-model="formData.lastName"
                :maxlength="50"
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
                clearable
                bg-color="grey-3"
                placeholder="9XXXXXXXXX"
                :maxlength="10"
                class="col col-12-xs col-12-sm col-12-md rounded-borders"
                v-model="formData.mobile"
                :rules="[mobileFormat, required, mobileLength]"
                style="max-width:350px">
                <template v-slot:prepend>
                  <span class="text-caption">+63</span>
                </template>
              </q-input>
            </div>
          </div>
        </q-card-section>
        <q-separator inset class="q-mt-sm q-mb-sm"/>
        <q-card-actions align="center" class="q-pt-xs q-pb-md">
            <q-btn
                color="secondary" label="Submit"
                class="text-capitalize q-px-xl full-width q-mb-xs"
                :disabled="isLoading"
                :loading="isLoading"
                @click="submit"
                style="max-width:360px"/>
            <q-btn
                flat
                label="Close"
                unelevated
                color="secondary"
                class="text-capitalize q-px-xl bg-grey-2 full-width q-mt-sm"
                :disabled="isLoading"
                @click="$emit('close')"
                style="max-width:360px"/>
        </q-card-actions>
    </q-card>
    <q-card v-if="responseMsg !== ''">
      <q-card-section  class="text-dbg text-center">
          <span class="text-bold text-h6 text-dbg"
            v-if="responseMsg === 'going'">Hey, we're excited <br/> to celebrate with you!</span>
          <span class="text-dbg text-h6"
            v-if="responseMsg === 'sorry'">Thanks for letting us know!</span>
      </q-card-section>
      <q-card-section  class="text-dbg text-center q-pt-none">
          <span class="text-dbg"
            v-if="responseMsg === 'going'">Message us on Facebook <br/>or Instagram if you have questions.</span>
          <span class="text-dbg"
            v-if="responseMsg === 'sorry'">I wish you could be there, but we truly understand. <br/>In case you change your mind, you still have until March 12 to message us.</span>
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
    // eslint-disable-next-line prefer-regex-literals
    const mobilePattern = new RegExp(/\d{10}$/gm)

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

      if (formData.lastName.length > 40 || formData.firstName.length > 40) {
        $q.notify({
          message: 'Unable to submit. Only a maximum of 40 characters is allowed.',
          multiLine: true,
          color: 'negative',
          timeout: 2200
        })
        return false
      }

      const mobile = parseInt(formData.mobile)
      if ((/[a-zA-Z]/).test(formData.mobile) || mobile === 0 || mobile.toString().length < 10) {
        $q.notify({
          message: 'Please check the format of your mobile number.',
          multiLine: true,
          color: 'negative',
          timeout: 2200
        })
        return false
      }

      isLoading.value = true

      /** Check if mobile number is existing */
      const response = await supabase
        .from('rsvp')
        .select('mobile_number, id, first_name, last_name, is_going')
        .eq('mobile_number', `+63${formData.mobile.trim()}`)

      if (response.error) {
        $q.notify({
          message: `Ooops! An error occurred while processing your reply. Please try again. (Error: ${response.statusText})`,
          multiLine: true,
          color: 'negative',
          timeout: 2300
        })
        return false
      }

      const data = response.data

      /** If existing: update */
      if (data.length > 0 && data.length === 1) {
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
          $q.notify({
            message: `Ooops! An error occurred while processing your reply. Please try again. (Error: ${updateResponse.statusText})`,
            multiLine: true,
            color: 'negative',
            timeout: 2300
          })
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

      /** else: add */
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
        $q.notify({
          message: `Ooops! An error occurred while processing your reply. Please try again. (Error: ${addResponse.statusText})`,
          multiLine: true,
          color: 'negative',
          timeout: 2300
        })
        return
      }

      if (formData.isGoing === 'Going') {
        responseMsg.value = 'going'
      } else {
        responseMsg.value = 'sorry'
      }
    }

    const mobileFormat = val => mobilePattern.test(val) || 'Please check the format of your mobile number.'
    const required = val => !!val || 'Mobile number is required'
    const mobileLength = val => val.length < 11 || 'Character limit reached.'
    const nameLength = val => val.length < 41 || 'Maximum of 40 characters only.'

    return {
      mobileFormat,
      required,
      nameLength,
      mobileLength,
      responseMsg,
      formData,
      isLoading,
      submit
    }
  }
})
</script>

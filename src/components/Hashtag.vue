<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'Hashtag'
})
</script>

<template>
    <div class="bg-lblue q-py-lg q-px-lg text-dbg q-pb-xl"
        id="hashtag">
        <p class="text-h5">
            Capture all the love!
        </p>
        <p>
            Don't forget to take photos and videos of all the fun moments from our wedding!
        </p>
        <div class="q-mt-md">
            <fieldset class="text-center q-pa-none"
                style="width:auto;margin:auto;border-color:#415a70;width:300px">
                <legend class="text-center q-px-sm">Please use the hashtag</legend>
                <div class="text-h4">
                    #RoyMariel2022
                </div>
                <div class="text-center q-px-sm">and tag us on FB or IG.</div>
            </fieldset>
        </div>
    </div>
</template>

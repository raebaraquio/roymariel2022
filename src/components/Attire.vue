
<template>
    <div class="bg-lblue q-pt-lg q-pb-xl text-dbg q-px-md"
        id="attire">
        <p class="text-center q-my-none">
            Please come in
        </p>
        <span class="text-h5 text-center q-mt-none">Formal Garden Attire</span>

        <div class="row no-padding">
            <q-card flat square
                class="col text-right text-dbg bg-lblue">
                <q-card-section class="text-rigt">
                    <span class="text-h6 text-dbg text-uppercase">Gentlemen</span> <br/>
                    <span class="text-bold text-h6 text-dbg">Coat and Tie or Long Sleeves Polo</span> <br />
                    <span>No jeans, rubber shoes or sneakers</span>
                </q-card-section>
            </q-card>

            <q-card flat square
                class="col text-left text-dbg bg-lblue">
                <q-card-section class="text-left">
                    <span class="text-h6 text-dbg text-uppercase">Ladies</span> <br/>
                    <span class="text-bold text-h6 text-dbg">Maxi/Long Dress or Cocktail Dress</span> <br />
                    <span>No pants or jumpsuit</span>
                </q-card-section>
            </q-card>
        </div>
        <div class="text-dbg text-center">
            <div class="row colors-bg rounded-borders q-px-md q-py-xs"
                style="max-width: 80vw;margin:auto; border-radius:1rem">
                <div class="col">
                    <span class="text-h6 text-uppercase"
                    style="letter-spacing:2px">Colors</span>
                </div>
                <div class="col text-left q-pt-xs q-gutter-xs">
                <q-btn round flat size="xs" class="color-1"></q-btn>
                <q-btn round flat size="xs" class="color-2"></q-btn>
                <q-btn round flat size="xs" class="color-3"></q-btn>
                <q-btn round flat size="xs" class="color-4"></q-btn>
                <q-btn round flat size="xs" class="color-5"></q-btn>
                </div>
            </div>
        </div>
        <p class="text-center q-mt-md">
            We'll be at an outdoor garden venue, so wear comfy footwear, like flats or block heels!
        </p>
    </div>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'Attire'
})
</script>


<template>
    <div class="bg-lblue q-pt-lg q-pb-xl text-dbg q-px-md"
        id="attire">
        <p class="text-center text-bold q-my-none">
            Please come in
        </p>
        <span class="text-h5 text-center q-mt-none">Formal Garden Attire</span>
        <div class="q-mt-lg">
            <q-img src="Attire.png"
                style="max-width:140px"/>
        </div>

        <div class="row no-padding q-mt-sm">
            <q-card flat square
                class="col text-right no-padding text-dbg bg-lblue">
                <q-card-section class="text-right q-pl-none">
                    <span class="text-dbg text-uppercase" style="letter-spacing:1px;">Gentlemen</span> <br/>
                    <span class="text-bold text-dbg" style="font-size:1.05rem;">Suit and Tie or <br/>Long Sleeves Polo</span> <br />
                    <span class="text-caption">No jeans, rubber <br/>shoes or sneakers</span>
                </q-card-section>
            </q-card>

            <q-separator vertical inset />

            <q-card flat square
                class="col text-left text-dbg bg-lblue">
                <q-card-section class="text-left q-pr-none">
                    <span class="text-dbg text-uppercase" style="letter-spacing:1px;">Ladies</span> <br/>
                    <span class="text-bold text-dbg" style="font-size:1.05rem;">Maxi/Long Dress <br/>or Cocktail Dress</span> <br />
                    <span class="text-caption">No pants or <br/>jumpsuit</span>
                </q-card-section>
            </q-card>
        </div>
        <div class="text-dbg text-center row q-py-md">
            <div class="col-9 col-md-4 col-sm-10 col-xs-11 row colors-bg rounded-borders q-px-md q-py-xs"
                style="margin:auto; border-radius:1rem">
                <div class="col">
                    <div class="text-h6 text-uppercase"
                    style="letter-spacing:2px">Colors</div>
                </div>
                <div class="lt-md col text-left q-pt-xs q-gutter-xs">
                    <q-btn round flat size="xs" class="color-1"></q-btn>
                    <q-btn round flat size="xs" class="color-2"></q-btn>
                    <q-btn round flat size="xs" class="color-3"></q-btn>
                    <q-btn round flat size="xs" class="color-4"></q-btn>
                    <q-btn round flat size="xs" class="color-5"></q-btn>
                </div>
                <div class="gt-sm col text-left q-pt-xs q-gutter-sm">
                    <q-btn round flat size="xs" class="color-1"></q-btn>
                    <q-btn round flat size="xs" class="color-2"></q-btn>
                    <q-btn round flat size="xs" class="color-3"></q-btn>
                    <q-btn round flat size="xs" class="color-4"></q-btn>
                    <q-btn round flat size="xs" class="color-5"></q-btn>
                </div>
            </div>
        </div>
        <p class="text-center q-mt-md">
            We'll be at an outdoor garden venue, so wear <br/>comfy footwear, like flats or block heels!
        </p>
    </div>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'Attire'
})
</script>

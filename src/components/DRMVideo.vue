<script setup lang="ts">
import { onMounted, onBeforeUnmount, ref } from 'vue'
import videojs from 'video.js'
import 'video.js/dist/video-js.css'
import 'videojs-contrib-eme'

const videoPlayer = ref<HTMLElement | null>(null)
let player: any = null
//https://entitlement-service.dev.grapeseed.com/api/license-message/33126bc7-7b75-4435-a232-40b84359b7a6
const token =
  "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ2ZXJzaW9uIjoxLCJjb21fa2V5X2lkIjoiODIyOGEwMDUtMjk0OS00ZmU4LWEwY2EtYjIzNDAwYjE3NDYwIiwibWVzc2FnZSI6eyJ0eXBlIjoiZW50aXRsZW1lbnRfbWVzc2FnZSIsInZlcnNpb24iOjIsImxpY2Vuc2UiOnsiZHVyYXRpb24iOjgsImFsbG93X3BlcnNpc3RlbmNlIjp0cnVlLCJmZWF0dXJlcyI6WyJmZWF0dXJlMSIsImZlYXR1cmUyIiwiZmVhdHVyZTMiXSwiZmFpcnBsYXkiOnsicmVhbF90aW1lX2V4cGlyYXRpb24iOnRydWV9LCJwbGF5cmVhZHkiOnsicmVhbF90aW1lX2V4cGlyYXRpb24iOnRydWV9fSwiY29udGVudF9rZXlzX3NvdXJjZSI6eyJpbmxpbmUiOlt7ImlkIjoiOWJjMjg3NmUtYjVlMi04NmEzLTcyY2ItYjVmMzRlOGE5MjIwIn1dfX19.GZ5-y9XYekNfIo6pJ4T-UNdnFFS0XfYmCfA3wUji4eU"
const streamingUrl = 'https://d1l43lr4xe74fx.cloudfront.net/33126bc7-7b75-4435-a232-40b84359b7a6/MonkeyMonkey.m3u8'
// const streamingUrl = '/videos/monkey/MonkeyMonkey.mpd'
onMounted(() => {
  if (!videoPlayer.value) return
  player = videojs(videoPlayer.value, {
    controls: true,
    autoplay: false,
    preload: 'auto',
  })

  player.eme()

  player.ready(() => {
    player!.src({
      src: streamingUrl,
      type: 'application/x-mpegURL', //m3u8
      // type: 'application/dash+xml', //mpd
      keySystems: {
        //PlayReady API (Windows/Edge/IE):
        'com.microsoft.playready': {
          url: 'https://drm-playready-licensing.axprod.net/AcquireLicense',
          licenseHeaders: {
            'X-AxDRM-Message': token,
          },
        },
        // //Widevine API (Chrome/Android):
        'com.widevine.alpha': {
          url: 'https://drm-widevine-licensing.axprod.net/AcquireLicense',
          licenseHeaders: {
            'X-AxDRM-Message': token,
          },
        },
        // 'com.apple.fps.1_0': {
        //   certificateUri: 'https://vtb.axinom.com/FPScert/fairplay.cer',
        //   licenseUri: 'https://drm-fairplay-licensing.axprod.net/AcquireLicense',
        //   licenseHeaders: {
        //     'X-AxDRM-Message': token,
        //   },
        //   // getContentId: function (emeOptions, initData) {
        //   //   return new TextDecoder().decode(initData.filter(item => item !== 0 && item !== 150))
        //   // },
        // },
        // 'org.w3.clearkey': {
        //   url: "https://drm-clearkey-licensing.axprod.net/AcquireLicense",
        //   licenseHeaders: {
        //     "X-AxDRM-Message": token,
        //   },
        // },
      },
    })
  })

  player.on('error', (error: unknown) => {
    console.error('Video.js error:', error)
  })
})

onBeforeUnmount(() => {
  if (player) {
    player.dispose()
  }
})
</script>

<template>
  <div class="relative flex justify-center items-center rounded-lg overflow-hidden">
    <video ref="videoPlayer" class="video-js vjs-default-skin" width="600" height="300"></video>
  </div>
</template>

<style>
/* Add any additional styles here */
</style>

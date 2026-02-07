<template>
<v-container class="fill-height d-flex align-center justify-center">
  <div class="d-flex flex-column align-center">
    <v-alert
      v-model="alert"
      color="warning"
      icon="$warning"
      :text="warningMessage"
      class="mb-4"
      max-width="500"
    />

    <v-card
      min-width="500"
      variant="outlined"
      color="pink-accent-1"
      title="be my valentine?"
    >
      <v-img
        :src="currentGif"
        height="300"
        width="300"
        class="ml-12"
      />

      <v-card-actions class="float-right mb-5">
        <v-btn
          variant="tonal"
          color="pink-accent-1"
          :style="yesButtonStyle"
          @click="yesClicked"
        >
          Yes
        </v-btn>

        <v-btn
          variant="outlined"
          color="pink-accent-1"
          @click="noClicked"
          :disabled="noDisabled"
        >
          No :(
        </v-btn>
      </v-card-actions>
    </v-card>
  </div>
</v-container>


</template>

<script setup lang="ts">
import { ref, watch, computed } from 'vue'
import confetti from 'canvas-confetti'

const alert = ref(false)
const noClickCount = ref(0)
const warningMessage = ref('')
const noDisabled = ref(false)
const accepted = ref(false)

const gifs = {
  begging: new URL('../assets/200w.gif', import.meta.url).href,
  happy: new URL('../assets/c616fca52915df67140d1ce0e002ce5a.gif', import.meta.url).href
}

const currentGif = computed(() =>
  accepted.value ? gifs.happy : gifs.begging
)

const messages = [
  'u sure u dont wanna do facials w me?',
  'okay wow that kinda hurt 😔',
  'lemme tell u something!!!!',
  'plsss scissor w me',
  'ur done'
]

function noClicked() {
  noClickCount.value++
}

function yesClicked() {
  accepted.value = true
  alert.value = false

  confetti({
    particleCount: 150,
    spread: 80,
    origin: { y: 0.6 }
  })
}

watch(noClickCount, (count) => {
  alert.value = true
  warningMessage.value =
    messages[Math.min(count - 1, messages.length - 1)]

  if (count >= 5) {
    noDisabled.value = true
  }
})

const yesButtonStyle = computed(() => {
  if (noClickCount.value >= 4) {
    return {
      transform: 'scale(2.5)',
      marginRight: '50px',
      marginBottom: '17px', // adjust as needed
      transition: 'transform 0.3s ease, margin 0.3s ease'
    }
  }
  return {}
})

</script>



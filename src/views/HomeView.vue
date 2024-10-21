<script setup lang="ts">
import HomeChild from "@/home/homeChild.vue";
import {provide, ref} from "vue";

const dialog = ref(false)
const hitMyEvent = () => {
  // console.log('hitMyEvent')
  dialog.value = true
}

provide('providing', 'providing value')

</script>

<template>
  <main class="pa-2">
<!--    <TheWelcome />-->
    <h1>
      This is a parent component.
    </h1>
    <HomeChild name="vikash" :age="23" @my-event="hitMyEvent">
      This is a slot in the child component.
      <template #namedSlot>
        <div class="text-subtitle-1 text-primary">
          named slot
        </div>
      </template>
    </HomeChild>

    <VDialog
      v-model="dialog"
      max-width="290"
    >
      <VCard>
        <VCardTitle class="headline">
          Dialog
        </VCardTitle>
        <VCardText>
          The dialog is now open
        </VCardText>
        <VCardActions>
          <VBtn
            color="primary"
            @click="dialog = false"
          >
            Close
          </VBtn>
        </VCardActions>
      </VCard>
    </VDialog>
  </main>
</template>

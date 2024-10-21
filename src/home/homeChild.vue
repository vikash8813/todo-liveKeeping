<script setup>

// interface Props {
//   name: string
//   age: string
// }
import {inject, ref} from "vue";

const props = defineProps({
  name: String,
  age: Number
})

const emit = defineEmits({
  myEvent: null
})

const value = inject('providing')


</script>

<template>
  <div>
    <h3 class="mt-2">
      My name is {{props.name}} and age is {{props.age}}
    </h3>
    <slot /> <br>
    <slot name="namedSlot" />
    <VBtn
      class="my-2"
      color="primary" @click="emit('myEvent')">
      Test
    </VBtn>

    <p>
      this is an injected value:
      <span class="font-weight-bold">
        {{value}}
      </span>
    </p>
  </div>
</template>


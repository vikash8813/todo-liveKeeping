<script setup>
import {ref} from "vue";

const props = defineProps({
  dialog: Boolean,
  type: String,
  todo: {
    type: Object,
  },
})

const todo = ref(JSON.parse(JSON.stringify(props.todo)))

const emits = defineEmits({
  save: (todo) => ({todo}),
  close: () => false,
})

const refForm = ref(null)

const onSave = () => {
  refForm.value.validate().then((valid) => {
    // console.warn(valid)
    if (valid.valid) {
      emits('save', todo.value)
    }
  })
}
</script>

<template>
  <VDialog
    :model-value="props.dialog"
    @update:model-value="emits('close')"
  >
    <VForm ref="refForm" @submit.prevent="onSave">
      <VCard>
      <VCardTitle>
        {{props.type}} Todo :
        <span>
          {{todo.id}}
        </span>
      </VCardTitle>
      <VCardText>
        <VRow>
          <VCol
            cols="12"
            >
            <VTextField
              label="Title"
              v-model="todo.title"
              :rules="[v => !!v || 'Title is required']"
              autofocus
            />
          </VCol>
          <VCol
            cols="12"
            >
            <VTextarea
              label="Description"
              v-model="todo.description"
            />
          </VCol>
        </VRow>
      </VCardText>
      <VCardActions>
        <VBtn
          color="primary"
          type="submit"
        >
          Save
        </VBtn>
        <VBtn
          color="error"
          @click="$emit('close')"
        >
          Close
        </VBtn>
      </VCardActions>
    </VCard>
    </VForm>
  </VDialog>
</template>

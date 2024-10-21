<script setup>

import {ref} from "vue";

const todo = defineProps(['todo','serial'])

const emit = defineEmits({
edit: Number,
delete: Number,
  update: (id,value) => ({id,value})
})

const test = ref(false)
</script>

<template>
  <div>
    <v-hover>
      <template v-slot:default="{ isHovering, props }">
        <VCard
          v-bind="props"
          :elevation="isHovering ? 10 : 2"
          class="d-flex align-center justify-space-between my-2 py-1 px-3">
          <div class="d-flex align-center">
            <VCheckbox
              v-model="todo.todo.completed"
              density="compact" :label="todo.serial.toString()"
              @update:model-value="emit('update',todo.todo.id,$event)"
            />
<!--            <h3 class="text-subtitle-1">{{todo.serial}}.</h3>-->
          </div>
          <h3 class="text-subtitle-1 text-capitalize">{{todo.todo.title}}</h3>
          <p>{{todo.todo.description}}</p>
          <div>
            <VBtn color="primary" variant="plain" @click="emit('edit',todo.todo.id)">Edit</VBtn>
            <VBtn color="error" variant="plain" class="ml-2" @click="emit('delete',todo.todo.id)">Delete</VBtn>
          </div>
        </VCard>
      </template>
    </v-hover>
  </div>


</template>

<style scoped>
::v-deep .v-input__details {
  display: none;
}
</style>

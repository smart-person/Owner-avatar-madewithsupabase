<script setup lang="ts">
import type { PropType } from 'vue'

const props = defineProps({
  modelValue: Object as PropType<{ handler: string; email: string }[]>,
})

const emits = defineEmits(['update:modelValue'])

function addMember() {
  emits('update:modelValue', [
    ...(props.modelValue ?? []),
    { handler: '', email: '' },
  ])
}

function removeMember(index: number) {
  if (!props.modelValue?.length)
    return
  props.modelValue.splice(index, 1)
  emits('update:modelValue', props.modelValue)
}
</script>

<template>
  <div v-if="modelValue?.length">
    <div class="flex flex-col md:flex-row">
      <label class="md:w-1/2 mr-4">Member's Twitter/GitHub</label>
      <label class="md:w-1/2">Member's Email</label>
    </div>

    <div
      v-for="(member, index) in modelValue"
      class="flex flex-col md:flex-row items-center mb-1"
    >
      <div class="flex items-center w-full md:w-1/2 md:mr-4">
        <button class="text-xl mr-2" @click.prevent="removeMember(index)">
          <div class="i-mdi-close" />
        </button>
        <span class="text-xl mr-2">@</span>
        <input
          v-model="member.handler"
          class="w-full"
          name="twitter"
          type="text"
          placeholder="user_handler"
        >
      </div>
      <div class="w-full md:w-1/2 pl-13 md:pl-0">
        <input
          v-model="member.email"
          name="email"
          class="w-full"
          type="text"
          placeholder="user@gmail.com"
        >
      </div>
    </div>
  </div>

  <button
    class="btn mt-4 w-max flex items-center"
    :disabled="modelValue && modelValue?.length > 3"
    @click.prevent="addMember"
  >
    <div class="i-mdi-plus mr-2" />
    Add a member
  </button>
</template>

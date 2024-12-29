<template>
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
    <div
      v-for="(note, index) in notes"
      :key="index"
      class="bg-white dark:bg-gray-800 p-4 rounded-lg shadow-lg hover:shadow-xl transition-shadow"
    >
      <p class="text-gray-700 dark:text-gray-300">{{ note.text }}</p>
      <div class="mt-4 flex justify-end space-x-2">
        <button
          @click="$emit('edit-note', note)"
          class="text-blue-500 hover:text-blue-600"
        >
          <PencilIcon class="h-5 w-5" />
        </button>
        <button
          @click="$emit('delete-note', index)"
          class="text-red-500 hover:text-red-600"
        >
          <TrashIcon class="h-5 w-5" />
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { PencilIcon, TrashIcon } from "@heroicons/vue/24/outline";

export default {
  props: {
    notes: {
      type: Array,
      required: true,
    },
  },
  components: {
    PencilIcon,
    TrashIcon,
  },
};
</script>

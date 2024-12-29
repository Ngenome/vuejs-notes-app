<template>
  <div class="bg-white dark:bg-gray-800 p-4 shadow-lg rounded-lg">
    <!-- Toggle Button -->
    <button
      @click="isExpanded = !isExpanded"
      class="w-full flex items-center justify-between focus:outline-none"
    >
      <span class="text-lg font-semibold text-gray-800 dark:text-white"
        >Add a Note</span
      >
      <ChevronDownIcon
        :class="[
          'h-5 w-5 text-gray-600 dark:text-gray-300 transform transition-transform',
          isExpanded ? 'rotate-180' : '',
        ]"
      />
    </button>

    <!-- Collapsible Form -->
    <div v-if="isExpanded" class="mt-4 space-y-4">
      <div>
        <label
          for="noteText"
          class="block text-sm font-medium text-gray-700 dark:text-gray-300"
          >Note</label
        >
        <div class="mt-1 relative rounded-md shadow-sm">
          <textarea
            v-model="noteText"
            id="noteText"
            placeholder="Enter your note..."
            required
            maxlength="500"
            class="w-full p-3 border border-gray-300 dark:border-gray-600 rounded-md focus:ring-2 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:text-white resize-none"
            rows="4"
          ></textarea>
          <span class="absolute bottom-2 right-2 text-sm text-gray-400">
            {{ 500 - noteText.length }} characters left
          </span>
        </div>
      </div>
      <div>
        <label
          for="category"
          class="block text-sm font-medium text-gray-700 dark:text-gray-300"
          >Category</label
        >
        <div class="mt-1 relative rounded-md shadow-sm">
          <select
            v-model="category"
            id="category"
            required
            class="w-full p-3 border border-gray-300 dark:border-gray-600 rounded-md focus:ring-2 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:text-white appearance-none"
          >
            <option value="" disabled>Select Category</option>
            <option v-for="cat in categories" :key="cat" :value="cat">
              {{ cat }}
            </option>
          </select>
          <span class="absolute inset-y-0 right-0 pr-3 flex items-center">
            <TagIcon class="h-5 w-5 text-gray-400" />
          </span>
        </div>
      </div>
      <button
        type="submit"
        class="w-full flex justify-center items-center bg-blue-600 text-white py-3 px-4 rounded-md hover:bg-blue-700 focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 transition-all"
      >
        <PlusIcon class="h-5 w-5 mr-2" />
        Add Note
      </button>
    </div>
  </div>
</template>

<script>
import { ChevronDownIcon, TagIcon, PlusIcon } from "@heroicons/vue/24/outline";

export default {
  props: {
    categories: {
      type: Array,
      required: true,
    },
  },
  components: {
    ChevronDownIcon,
    TagIcon,
    PlusIcon,
  },
  data() {
    return {
      isExpanded: false,
      noteText: "",
      category: "",
    };
  },
  methods: {
    submitNote() {
      const note = {
        text: this.noteText,
        category: this.category,
      };
      this.$emit("add-note", note);
      this.noteText = "";
      this.category = "";
      this.isExpanded = false; // Collapse the form after submission
    },
  },
};
</script>

<template>
  <div
    class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center"
  >
    <div class="bg-white dark:bg-gray-800 p-6 rounded-lg shadow-lg w-96">
      <h2 class="text-lg font-bold text-gray-800 dark:text-white mb-4">
        Add Category
      </h2>
      <input
        v-model="newCategory"
        type="text"
        placeholder="Enter a new category"
        class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-md focus:ring-2 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:text-white"
      />
      <div class="mt-4 flex justify-end space-x-2">
        <button
          @click="$emit('close')"
          class="px-4 py-2 text-gray-700 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-700 rounded-md"
        >
          Cancel
        </button>
        <button
          @click="addCategory"
          class="px-4 py-2 bg-blue-600 text-white rounded-md hover:bg-blue-700"
        >
          Add
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newCategory: "",
    };
  },
  methods: {
    addCategory() {
      if (this.newCategory.trim()) {
        this.$emit("add-category", this.newCategory.trim());
        this.$emit("close");
      }
    },
  },
};
</script>

<template>
  <div
    id="app"
    :class="{ dark: isDarkMode }"
    class="min-h-screen bg-gray-50 dark:bg-gray-900"
  >
    <div class="flex">
      <!-- Sidebar -->
      <div class="w-64 bg-white dark:bg-gray-800 shadow-lg p-4">
        <h2 class="text-lg font-bold text-gray-800 dark:text-white mb-4">
          Notes App
        </h2>
        <button
          @click="openCategoryDialog"
          class="w-full flex items-center justify-center bg-blue-600 text-white py-2 px-4 rounded-md hover:bg-blue-700 mb-4"
        >
          <PlusIcon class="h-5 w-5 mr-2" />
          Add Category
        </button>
        <div class="space-y-2">
          <button
            v-for="cat in categories"
            :key="cat"
            @click="activeCategory = cat"
            :class="[
              'w-full text-left px-4 py-2 rounded-md focus:outline-none',
              activeCategory === cat
                ? 'bg-blue-600 text-white'
                : 'text-gray-700 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-700',
            ]"
          >
            {{ cat }}
          </button>
        </div>
        <div class="mt-6">
          <input
            v-model="searchQuery"
            type="text"
            placeholder="Search notes..."
            class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-md focus:ring-2 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:text-white"
          />
        </div>
      </div>

      <!-- Main Content -->
      <div class="flex-1 p-6">
        <h1 class="text-3xl font-bold text-gray-800 dark:text-white mb-6">
          My Notes
        </h1>
        <note-form :categories="categories" @add-note="addNote" />
        <note-list
          :notes="filteredNotes"
          @delete-note="deleteNote"
          @edit-note="openEditDialog"
        />
      </div>
    </div>

    <!-- Category Dialog -->
    <category-dialog
      v-if="isCategoryDialogOpen"
      @close="isCategoryDialogOpen = false"
      @add-category="addCategory"
    />

    <!-- Edit Dialog -->
    <edit-dialog
      v-if="isEditDialogOpen"
      :note="selectedNote"
      @close="isEditDialogOpen = false"
      @save="updateNote"
    />
  </div>
</template>

<script>
import { PlusIcon } from "@heroicons/vue/24/outline";
import NoteForm from "./components/NoteForm.vue";
import NoteList from "./components/NoteList.vue";
import CategoryDialog from "./components/CategoryDialog.vue";
import EditDialog from "./components/EditDialog.vue";

export default {
  components: {
    NoteForm,
    NoteList,
    CategoryDialog,
    EditDialog,
    PlusIcon,
  },
  data() {
    return {
      notes: JSON.parse(localStorage.getItem("notes")) || [],
      categories: JSON.parse(localStorage.getItem("categories")) || [
        "Work",
        "Personal",
        "Ideas",
      ],
      activeCategory: "Work",
      searchQuery: "",
      isDarkMode: false,
      isCategoryDialogOpen: false,
      isEditDialogOpen: false,
      selectedNote: null,
    };
  },
  computed: {
    filteredNotes() {
      return this.notes
        .filter((note) => note.category === this.activeCategory)
        .filter((note) =>
          note.text.toLowerCase().includes(this.searchQuery.toLowerCase())
        );
    },
  },
  methods: {
    addNote(note) {
      this.notes.push(note);
      this.saveNotes();
    },
    deleteNote(index) {
      this.notes.splice(index, 1);
      this.saveNotes();
    },
    openEditDialog(note) {
      this.selectedNote = note;
      this.isEditDialogOpen = true;
    },
    updateNote(updatedNote) {
      const index = this.notes.findIndex((note) => note.id === updatedNote.id);
      this.notes.splice(index, 1, updatedNote);
      this.saveNotes();
      this.isEditDialogOpen = false;
    },
    openCategoryDialog() {
      this.isCategoryDialogOpen = true;
    },
    addCategory(category) {
      if (!this.categories.includes(category)) {
        this.categories.push(category);
        localStorage.setItem("categories", JSON.stringify(this.categories));
      }
    },
    toggleDarkMode() {
      this.isDarkMode = !this.isDarkMode;
    },
    saveNotes() {
      localStorage.setItem("notes", JSON.stringify(this.notes));
    },
  },
};
</script>

<style>
.dark {
  background-color: #1a202c;
  color: #e2e8f0;
}
</style>

<template>
  <div id="app" class="min-h-screen bg-gray-100 p-6">
    <h1 class="text-3xl font-bold text-center mb-6">Notes Application</h1>
    <note-form @add-note="addNote" />
    <note-list :notes="notes" @delete-note="deleteNote" />
  </div>
</template>

<script>
import NoteForm from "./NoteForm.vue";
import NoteList from "./NoteList.vue";

export default {
  components: { NoteForm, NoteList },
  data() {
    return {
      notes: JSON.parse(localStorage.getItem("notes")) || [],
    };
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
    saveNotes() {
      localStorage.setItem("notes", JSON.stringify(this.notes));
    },
  },
};
</script>

<style>
/* Empty because Tailwind will handle styling */
</style>

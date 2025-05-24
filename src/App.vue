<script setup>
import { ref, watch, computed, onMounted } from "vue";
import Sidebar from "./components/Sidebar.vue";

const notes = ref([]);
const active_note = ref(null);
const input_title = ref("");
const input_content = ref("");
const search = ref("");

onMounted(() => {
  const saved = localStorage.getItem("notes");
  if (saved) {
    notes.value = JSON.parse(saved);
  }
});

watch(
  notes,
  () => {
    localStorage.setItem("notes", JSON.stringify(notes.value));
  },
  { deep: true }
);

function createNote() {
  const id = Math.random().toString(36).substring(2, 9);
  const timestamp = Date.now();
  notes.value.push({ id, title: "Untitled", content: "", updated: timestamp });
  set_active_note(id);
}

function set_active_note(id) {
  active_note.value = id;
  const note = notes.value.find((n) => n.id === id);
  if (note) {
    input_title.value = note.title;
    input_content.value = note.content;
    setTimeout(() => {
      document.querySelector("input[type='text']")?.focus();
    }, 0);
  }
}

function delete_note(id) {
  const index = notes.value.findIndex((n) => n.id === id);
  if (index !== -1) {
    notes.value.splice(index, 1);
    if (active_note.value === id) {
      active_note.value = notes.value.length ? notes.value[0].id : null;
      if (active_note.value) set_active_note(active_note.value);
    }
  }
}

function updateSearch(val) {
  search.value = val;
}

watch([input_title, input_content], ([newTitle, newContent]) => {
  const note = notes.value.find((n) => n.id === active_note.value);
  if (note) {
    note.title = newTitle;
    note.content = newContent;
    note.updated = Date.now();
  }
});

const filtered_notes = computed(() =>
  notes.value
    .filter((n) => n.title.toLowerCase().includes(search.value.toLowerCase()))
    .sort((a, b) => b.updated - a.updated)
);
</script>

<template>
  <div
    class="flex items-stretch justify-start min-h-screen text-white bg-fuchsia-800"
  >
    <Sidebar
      :active_note="active_note"
      :notes="filtered_notes"
      :search="search.value"
      @new-note="createNote"
      @set-active-note="set_active_note"
      @delete-note="delete_note"
      @update-search="updateSearch"
    />

    <main class="flex-1">
      <div class="flex flex-col h-full px-4 py-8" v-if="active_note">
        <input
          type="text"
          v-model="input_title"
          class="block w-full pb-2 text-3xl font-bold transition-colors duration-200 border-b-2 outline-none border-fuchsia-800 focus:border-b-fuchsia-50"
        />
        <textarea
          v-model="input_content"
          class="flex-1 block w-full h-full mt-4 text-lg bg-transparent outline-none resize-none"
        ></textarea>
      </div>
    </main>
  </div>
</template>

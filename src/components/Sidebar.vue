<script setup>
import { ref, watch } from "vue";

const props = defineProps(["active_note", "notes", "search"]);
const emit = defineEmits([
  "new-note",
  "set-active-note",
  "delete-note",
  "update-search",
]);

const model = ref(props.search);

watch(
  () => props.search,
  (val) => {
    model.value = val;
  }
);
</script>

<template>
  <aside class="w-[256px] min-h-screen bg-fuchsia-900">
    <div class="p-4 space-y-4">
      <button class="w-full button" @click="emit('new-note')">New Note</button>
      <input
        type="text"
        class="w-full p-2 text-white placeholder-gray-300 rounded bg-fuchsia-800 focus:outline-none"
        placeholder="Search notes..."
        v-model="model"
        @input="emit('update-search', model)"
      />
    </div>

    <ul>
      <li
        v-for="note in props.notes"
        :key="note.id"
        class="flex items-center justify-between gap-4 p-4 border-b cursor-pointer border-fuchsia-700 hover:bg-fuchsia-800"
        @click="emit('set-active-note', note.id)"
      >
        <span :class="{ 'font-bold': note.id === props.active_note }">
          {{ note.title }}
        </span>
        <button
          class="text-gray-400 hover:text-white"
          @click.stop="emit('delete-note', note.id)"
        >
          🗑
        </button>
      </li>
    </ul>
  </aside>
</template>

<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="run in runs"
      :key="run.id"
      :class="run.runningdistance < 0 ? 'minus' : 'plus'"
    >
      {{ run.text }} <span>{{ run.runningdistance }} miles</span
      ><button class="delete-btn" @click="deleteRun(run.id)">
        x
      </button>
    </li>
  </ul>
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps({
  runs: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(['runDeleted']);

const deleteRun = (id) => {
  emit('runDeleted', id);
};
</script>

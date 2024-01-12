<template>
  <Header />
  <div class="container">
    <Distance :distance="+distance" />
    <RunLog
      :runs="runs"
      @runDeleted="handleRunDeleted"
    />
    <AddNewRun @runSubmitted="handleRunSubmitted" />
  </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Distance from './components/Distance.vue';
import RunLog from './components/RunLog.vue';
import AddNewRun from './components/AddNewRun.vue';

import { ref, computed, onMounted } from 'vue';

import { useToast } from 'vue-toastification';

const toast = useToast();

const runs = ref([]);

onMounted(() => {
  const savedRuns = JSON.parse(localStorage.getItem('runs'));

  if (savedRuns) {
    runs.value = savedRuns;
  }
});

// Get runningdistance
const runningdistance = computed(() => {
  return runs.value.reduce((acc, run) => {
    return acc + run.runningdistance;
  }, 0);
});

// Get distance
const distance = computed(() => {
  return runs.value
    .filter((run) => run.runningdistance > 0)
    .reduce((acc, run) => acc + run.runningdistance, 0)
    .toFixed(2);
});


// Submit run
const handleRunSubmitted = (RunData) => {
  runs.value.push({
    id: generateUniqueId(),
    text: RunData.text,
    runningdistance: RunData.runningdistance,
  });

  saveRunsToLocalStorage();

  toast.success('Great job! Your latest run has been successfully recorded.');
};

// Generate unique ID
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
};

// Delete run
const handleRunDeleted = (id) => {
  runs.value = runs.value.filter(
    (run) => run.id !== id
  );

  saveRunsToLocalStorage();

  toast.success('Run log deleted.');
};

// Save runs to local storage
const saveRunsToLocalStorage = () => {
  localStorage.setItem('runs', JSON.stringify(runs.value));
};
</script>

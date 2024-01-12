<template>
  <h3>Log new run</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Date</label>
      <input type="date" id="date" name="new-run" min="2024-01-01" max="2099-12-31" v-model="date" />
    </div>
    <div class="form-control">
      <label for="runningdistance">Miles run in session: <br /> </label>
      <input
        type="text"
        id="runningdistance"
        placeholder="Enter distance (mi)"
        v-model="runningdistance"
      />
    </div>
    <button class="btn">Log New Run</button>
  </form>
</template>

<script setup>
import { useToast } from 'vue-toastification';
import { ref } from 'vue';

const date = ref('');
const runningdistance = ref('');

// Get toast interface
const toast = useToast();

const emit = defineEmits(['runSubmitted']);

const onSubmit = () => {
  if (!date.value || !runningdistance.value) {
    // Display a toast error message if either field is empty
    toast.error('Both fields are required to be completed.');
    return;
  }

  const runData = {
    text: date.value,
    runningdistance: parseFloat(runningdistance.value),
  };

  emit('runSubmitted', runData);

  // Clear form fields
  date.value = '';
  runningdistance.value = '';
};
</script>

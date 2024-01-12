<template>
  <div class="dist-container">
    <div>
      <h4>Miles Ran:</h4>
      <p id="miles-plus" class="miles plus">+{{ distance }} miles</p>
    </div>
  </div>
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps({
  distance: {
    type: Number,
    required: true,
  }
});
</script>

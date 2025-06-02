<script setup>
import { ref } from 'vue';

const currentDate = ref(new Date().toISOString().split('T')[0]); 
const showDatePicker = ref(false);

const toggleDatePicker = () => {
  showDatePicker.value = !showDatePicker.value;
};

const updateDate = (event) => {
  currentDate.value = event.target.value;
  showDatePicker.value = false;
};
</script>

<template>
  <div class="date-container">
    <span @click="toggleDatePicker" class="current-date">{{ currentDate }}</span>
    
    <input 
      v-if="showDatePicker" 
      type="date" 
      :value="currentDate" 
      @input="updateDate"
      class="date-picker"
    />
  </div>
</template>

<style scoped>
.date-container {
  text-align: right;
}

.current-date {
  cursor: pointer;
  font-weight: bold;
  padding: 8px;
  background: #f0f0f0;
  border-radius: 4px;
}

.date-picker {
  margin-top: 5px;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
</style>

<script setup>
import { ref } from 'vue';

const searchQuery = ref('');
const suggestions = ref([]);
const selectedCustomer = ref(null);

const emit = defineEmits(['select-customer']);

// Fetch customer names from API based on input
const fetchCustomers = async (event) => {
  searchQuery.value = event.target.value.trim();

  if (!searchQuery.value) {
    suggestions.value = [];
    return;
  }

  try {
    const response = await fetch(`https://dummyjson.com/users/search?q=${searchQuery.value}`);
    const data = await response.json();

    suggestions.value = data.users.map(user => ({
      id: user.id,
      name: `${user.firstName} ${user.lastName}`
    }));
  } catch (error) {
    console.error('Error fetching customers:', error);
  }
};

const selectCustomer = (customer) => {
  searchQuery.value = customer.name;
  selectedCustomer.value = customer.name;
  suggestions.value = [];
  emit('select-customer', customer);
};
</script>

<template>
  <div class="customer-search">
    <label class="label">Customer Name</label>

    <input 
      :value="searchQuery"
      @input="fetchCustomers"
      type="text" 
      placeholder="Search customer..." 
      class="search-input"
    />

    <ul v-if="suggestions.length" class="suggestion-list">
      <li v-for="customer in suggestions" :key="customer.id" @click="selectCustomer(customer)">
        {{ customer.name }}
      </li>
    </ul>

    <p v-if="selectedCustomer" class="selected-name">{{ selectedCustomer }}</p>
  </div>
</template>


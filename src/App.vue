<script setup>
import { ref } from 'vue';
import CustomerSearch from './components/Customersearch.vue';
import DatePicker from './components/datePicker.vue';
import InvoiceNumber from './components/InvoiceNumber.vue';
import ItemsTable from './components/ItemsTable.vue';

const selectedCustomer = ref(null);

const handleCustomerSelect = (customer) => {
  selectedCustomer.value = customer;
};
</script>

<template>
  <div class="app-container">
    <div class="top-row">
    <div class="left-section">
      <h2>Billing Details</h2>
      <CustomerSearch @select-customer="handleCustomerSelect" />
    </div>

    <div class="right-section">
      <DatePicker />
    </div>
    
    </div>
    <div class="invoice-row">
      <hr>
      <InvoiceNumber />
    </div>
    <ItemsTable />
  </div>
</template>


<style scoped>
.app-container {
  max-width: 1200px;          
  margin: 50px 0 ;     
  text-align: left;        
  
}



.left-section {
  width: 50%;
  text-align: left;
}

.right-section {
  width: 150%;
  text-align: right;
}


.invoice-row {
  margin-top: 20px;
  clear: both; 
}

</style>
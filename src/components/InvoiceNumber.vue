<script setup>
import { ref } from 'vue';

const invoiceNumber = ref(generateInvoiceNumber());

function generateInvoiceNumber() {
  return Math.floor(1000 + Math.random() * 9000); 
}
</script>

<template>
  <div class="invoice-container">
    <label class="label-invoice">Invoice Number :<span class="invoice-number">
        {{ invoiceNumber }}</span></label>

  </div>
</template>





<script setup>
import { ref } from 'vue';

function createNewRow() {
  return {
    searchQuery: '',
    suggestions: [],
    itemName: '',
    quantity: 1,
    unitPrice: 0,
    total: 0
  };
}

const rows = ref([createNewRow()]);


function addRow() {
  rows.value.push(createNewRow());
}


function deleteRow(index) {
  rows.value.splice(index, 1);
}

async function fetchItems(row, event) {
  row.searchQuery = event.target.value.trim();
  
  if (!row.searchQuery) {
    row.suggestions = [];
    return;
  }
  
  try {
    const response = await fetch(`https://dummyjson.com/products/search?q=${row.searchQuery}`);
    const data = await response.json();
    row.suggestions = data.products || [];
  } catch (error) {
    console.error('Error fetching products:', error);
  }
}

function selectItem(row, product) {
  row.searchQuery = product.title;
  row.itemName = product.title;
  row.unitPrice = product.price;
  row.quantity = 1;
  row.total = row.unitPrice * row.quantity;
  row.suggestions = []; 
}

function updateQuantity(row) {
  row.total = row.quantity * row.unitPrice;
}
</script>

<template>
  <div class="items-table-container">
    <button class="add-btn" @click="addRow">+</button>

    <table class="items-table">
      <thead>
        <tr>
          <th>Items</th>
          <th>Quantity</th>
          <th>Unit Price</th>
          <th>Total</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, index) in rows" :key="index">
       
          <td>
            <input
              type="text"
              :value="row.searchQuery"
              @input="(event) => fetchItems(row, event)"
              placeholder="Search item..."
              class="item-search-input"
            />
           
            <ul v-if="row.suggestions.length" class="suggestions">
              <li 
                v-for="product in row.suggestions" 
                :key="product.id" 
                @click="selectItem(row, product)"
              >
                {{ product.title }}
              </li>
            </ul>
          </td>
 
          <td>
            <input 
              type="number" 
              min="1" 
              v-model="row.quantity" 
              @input="updateQuantity(row)" 
              class="quantity-input"
            />
          </td>

       
          <td>{{ row.unitPrice }}</td>

     
          <td>{{ row.total }} </td>

          <td> <button class="delete-btn" @click="deleteRow(index)"> Delete</button></td>

        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
.items-table-container {
  position: relative;
  margin-top: 20px;
  width: 100%;
}

.add-btn {
  position: absolute;
  top: -50px;
  right: -10px;
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 14px;
  font-size: 18px;
  cursor: pointer;
  border-radius: 50%;
  box-shadow: 0px 2px 4px rgba(0,0,0,0.2);
}
.add-btn:hover {
  background-color: #0056b3;
}


.items-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 30px;
}

.items-table thead {
  background-color: #f0f0f0;
}

.items-table th,
.items-table td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

.delete-btn {
  background-color: #d30f0f;
  color: rgb(215, 190, 190);
  border: none;
  padding: 6px 10px;
  font-size: 14px;
  cursor: pointer;
 
  
}
.delete-btn:hover {
  background-color: #c82333;
}


.item-search-input {
  width: 90%;
  padding: 6px;
  border: 1px solid #ccc;
  border-radius: 4px;
}


.suggestions {
  list-style: none;
  margin: 5px 0 0;
  padding: 0;
  background: #fff;
  border: 1px solid #ccc;
  border-radius: 4px;
  max-height: 150px;
  overflow-y: auto;
  position: absolute;
  width: 90%; 
  z-index: 999;
}

.suggestions li {
  padding: 6px;
  cursor: pointer;
  transition: background 0.2s;
}

.suggestions li:hover {
  background: #007bff;
  color: #fff;
}


.quantity-input {
  width: 60px;
  text-align: right;
}
</style>

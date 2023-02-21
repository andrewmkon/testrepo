<script setup lang="ts">
// @ts-nocheck
import {reactive, ref, provide} from 'vue'
import EditDialog from './EditDialog.vue'

const units = ['кг', 'л', 'шт'];
provide('units', units);

const editedProduct = ref(null);
var isNewProduct = false;

const products = reactive([
{
    name: 'Борошно',
    unit: 0,
    barcode: '4894401382164'
  },
  {
    name: 'Олiя',
    unit: 1,
    barcode: '8718599738267'
  }
]);


function addNew() {
  isNewProduct = true;
  editedProduct.value = {
    name: '',
    unit: '',
    barcode: ''
  };
}


function edit(product) {
  isNewProduct = false;
  editedProduct.value = product;
}


function onProductEdited(newProductObj) {
  if(isNewProduct)
    products.push(newProductObj);
  else
  {
    let index = products.indexOf(editedProduct.value);
    products[index] = newProductObj;
  }
  editedProduct.value = null;
}


function onEditCancelled() {
  editedProduct.value = null;
}
</script>

<template>
  <div>
    <table class="products">
      <thead>
        <tr>
          <th>Назва</th>
          <th>Од. вимiру</th>
          <th>Штрих-код</th>
        </tr>
      </thead>
      <tr v-for="p of products" :key="p.barcode">
        <td>{{p.name}}</td>
        <td>{{units[p.unit]}}</td>
        <td>{{p.barcode}}</td>
        <td>
          <button @click="edit(p)">Редагувати</button>
        </td>
      </tr>
    </table>
    
    <button @click="addNew">Додати товар</button>
    
    <EditDialog 
        v-if="editedProduct"
        :editedProduct="editedProduct"
        @done="onProductEdited"
        @cancel="onEditCancelled"
    />

  </div>
</template>

<style lang="scss" scoped>
.products {
  border-collapse: collapse;
  table-layout: fixed;
  text-align: left;
  margin-bottom: 8px;
  td, th {padding: 2px}
  thead {
    th:nth-child(1) {width: 160px;}
    th:nth-child(2) {width: 100px;}
    th:nth-child(3) {width: 120px;}
    th:nth-child(4) {width: 100px;}
  }
}
</style>

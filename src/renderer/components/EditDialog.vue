<script setup lang="ts">
// @ts-nocheck
import {inject, ref, watch} from 'vue'

const props = defineProps({
  editedProduct: Object
});

const emit = defineEmits(['done', 'cancel']);

const units = inject('units');

const product = ref({});
watch(() => props.editedProduct,
  () => product.value = {...props.editedProduct},
  {immediate: true});

const error = ref(null);

function done() {
  if(product.value.name.trim() == '')
    error.value = 'Введiть назву';
  else if(product.value.unit === '')
    error.value = 'Оберiть одиницю вимiру';
  else if(!product.value.barcode.match(/^\d{13,13}$/))
    error.value = 'Штрих-код має складатися з 13 цифр';
  else
    error.value = null;

  if(!error.value)
    emit('done', product.value);
}
</script>

<template>
<Teleport to="body">
  <div class="container">
    <form>
      <input type="text" placeholder="Назва" v-model="product.name" />

      <select v-model="product.unit">
        <option disabled value="">Одиницi</option>
        <option v-for="(unitName, i) of units" :value="i">{{ unitName }}</option>
      </select>

      <input type="text" maxlength="13" placeholder="Штрихкод" v-model="product.barcode" />
      
      <div class="error">{{ error }}</div>

      <div class="buttons">
        <button @click.prevent="emit('cancel')">Скасувати</button>
        <button @click.prevent="done">Зберегти</button>
      </div>
    </form>
  </div>
</Teleport>
</template>

<style lang="scss" scoped>
.container {
  position: fixed;
  left: px; top: 0px;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
form {
  background-color: #0004;
  padding: 16px;
  width: 200px;
  display: flex;
  flex-flow: column;
  gap: 16px;
  border-radius: 4px;
  input, select {height: 1.7em}
}
.error {color: red;}
.buttons {
  display: flex;
  justify-content: end;
  gap: 8px;
}
</style>

<template>
    <h3>History</h3>
    <ul id="list" class="list">
      <li class="plus" v-for="transation in transations" :key="transation.id" v-bind:class="transation.amount < 0 ? 'minus':'plus'">
        {{ transation.text }} <span>Rs. {{ transation.amount }}</span>
        <button class="delete-btn" @click="deleteTransaction(transation.id)">
          x
        </button>
      </li>
      
    </ul>
  </template>

<script setup>

const emit = defineEmits(['deletedTransation'])
const props = defineProps({
    transations: {
        type: Array,
        required: true
    }
})

const deleteTransaction =(id) =>{
    emit('deletedTransation',id)
}

</script>
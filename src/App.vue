<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpense :income="+income" :expense="+expense" />
    <Transaction :transations="transations" @deletedTransation="handleDeletedTransation"/>
    <AddTrasaction @transactionsubmitted="hanldetransactionsubmitted" />
    
  </div>
</template>

<script setup>
import Header from './components/header.vue'
import Balance from './components/balance.vue'
import IncomeExpense from './components/incomeExpense.vue'
import Transaction from './components/transaction.vue'
import AddTrasaction from './components/addTransaction.vue'
import { ref , computed , onMounted } from 'vue'
import { useToast } from 'vue-toastification'

const Toast = useToast()

const transations = ref([]);

// get total
const total = computed(()=> {
  return transations.value.reduce((acc, transaction) =>{
    return acc + transaction.amount;
  },0)
})

// get income
const income = computed(()=> {
  return transations.value.filter((transaction) => transaction.amount > 0).reduce((acc, transaction) =>{
    return acc + transaction.amount;
  },0)
})
// get expense
const expense = computed(()=> {
  return transations.value.filter((transaction) => transaction.amount < 0).reduce((acc, transaction) =>{
    return acc + transaction.amount;
  },0).toFixed(2)
})
// submitted data
const hanldetransactionsubmitted = (transactionData) => {
  transations.value.push({
    id: transations.value.length,
    text: transactionData.text,
    amount: transactionData.amount
  });
  Toast.success("Transaction added..!")
  savedTransationLocalstorage()
}
// delete transation
const handleDeletedTransation = (id) =>{
  transations.value = transations.value.filter((transaction) => transaction.id !== id)
  Toast.success("Transaction deleted..!")
  savedTransationLocalstorage()
}

onMounted(() => {
  const savedTransation = JSON.parse(localStorage.getItem('transactions'));
  if(savedTransation){
    transations.value = savedTransation;
  }
})
// save to loacalstorage
const savedTransationLocalstorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transations.value));
}
</script>
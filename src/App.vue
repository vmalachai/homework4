<script setup>
import Header from './components/Head.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import AddTransaction from './AddTransaction.vue';
import TransactionList from './components/TransactionList.vue';

const transactions = ref([])
import {ref,computed, onMounted} from 'vue'
const sum = computed(()=>{
    return transactions.value.reduce((acc, x)=>{
        return acc+x.amount
    },0)
} )
const moneyIn = computed(()=>{
    return transactions.value
    .filter((x)=>x.amount>0)
    .reduce((acc, x)=>{
        return acc+x.amount
    },0)
})
const moneyOut = computed(()=>{
    return transactions.value
    .filter((x)=>x.amount<0)
    .reduce((acc, x)=>{
        return acc+x.amount
    },0)
})

const handleTransaction = (transactionData) =>{
    transaction.value.push({
        id: generateID(),
        text: transactionData.text,
        amount: transactionData.amount,
    })
    saveToLocalStorage()

}
const generateID =() =>{
    return Math.floor(Math.random()*1000000)

}

const handleDelete = (id) => {
    transactions.value = transactions.value.filter((x) => x.id !== id)
}

const saveToLocalStorage = () => {
    localStorage.setitem('transactions', JSON.stringify(transaction.value))
}

onMounted(() => {
    const savedtransactions = JSON.parse(localStorage.getItem('transactions'))
    if(savedtransactions){
        transactions.value = savedtransactions
    }
})
</script>

<template>

<Header> </Header>
<div class="container">
    <Balance :total="sum"></Balance>
    <IncomeExpenses :income="moneyIn" :expense="moneyOut"> </IncomeExpenses>
    <AddTransaction @transactionSubmitted="handleTransaction"></AddTransaction>
    <TransactionList :transactions="transaction" @transactionDeleted="handleDelete"></TransactionList>
</div>
</template>
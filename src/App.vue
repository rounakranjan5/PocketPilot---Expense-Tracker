<script setup>

import { computed, ref ,onMounted} from 'vue';
import AddTransaction from './components/AddTransaction.vue';
import Balance from './components/Balance.vue';
import Header from './components/Header.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import { useToast } from 'vue-toastification';

const toast=useToast()

// const transactions=ref([
//     {id:1,text:'food',amount:-12999.00},
//     {id:2,text:'travel',amount:7500.00},
//     {id:3,text:'salary',amount:25000.00},
//     {id:4,text:'sports',amount:-2290.00},
// ])

const transactions=ref([])

onMounted(()=>{
    const savedTransactions=JSON.parse(localStorage.getItem('transactions'))

    if(savedTransactions){
        transactions.value=savedTransactions
    }
})

const total=computed(()=>{
    return transactions.value.reduce((acc,transaction)=>{
        return acc + transaction.amount;
    },0)
})

const income=computed(()=>{
    return transactions.value
    .filter((transaction)=>transaction.amount>0)
    .reduce((acc,transaction)=>{
        return acc + transaction.amount;
    },0).toFixed(2)
})

const expenses=computed(()=>{
    return transactions.value
    .filter((transaction)=>transaction.amount<0)
    .reduce((acc,transaction)=>{
        return acc + transaction.amount;
    },0).toFixed(2)
})

const generateUniqueId=()=>{
    return Math.floor(Math.random()*1000000)
}

const handleTransactionSubmitted=(transactionData)=>{
    transactions.value.push({
        id:generateUniqueId(),
        text:transactionData.text,
        amount:transactionData.amount
    })

    savedTransactionsToLocalStorage()

    toast.success('Transaction Added')
}

const handleTransactionDeleted=(id)=>{
    transactions.value=transactions.value.filter((transaction)=>transaction.id!==id);

    savedTransactionsToLocalStorage()

    toast.success('Transaction Deleted')
}

const savedTransactionsToLocalStorage=()=>{
    localStorage.setItem('transactions',JSON.stringify(transactions.value))
}


</script>

<template>

<Header></Header>
<div class="container">
<Balance :total="total" />
<IncomeExpense :income="+income" :expenses="-expenses"/>
<TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
<AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
</div>


</template>


<script setup>

import { ref } from 'vue';
import { useToast } from 'vue-toastification';
const text=ref('')
const amount=ref('')
const toast=useToast()

const emit=defineEmits(['transactionSubmitted'])
 
const onSubmit=()=>{
    if(!text.value || !amount.value){
        toast.error('Both fields must be filled')
        return ;
    }
    if(isNaN(amount.value)){
        toast.error('Amount must be a Valid Number')
        return;
    }
    const transactionData={
        text:text.value,
        amount:parseFloat(amount.value)
    }

    emit('transactionSubmitted',transactionData)

    text.value='';
    amount.value=''
}

</script>

<template>
    <h3>Add new transaction</h3>
    <form action="" id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" id="text" placeholder="Enter text..." v-model="text">
        </div>
        <div class="form-control">
            <label for="amount">Amount ( Enter Income in Postive and expenses in Negative)</label>
            <input type="text" id="amount" placeholder="Enter amount..." v-model="amount">
        </div>
        <button class="btn">Add transaction</button>
    </form>
</template>
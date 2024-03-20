<template>
    <h3>Add new transaction</h3>
    <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" v-model="transaction.text" id="text" placeholder="Enter text..." />
        </div>
        <div class="form-control">
            <label for="amount">Amount <br />
                (negative - expense, positive - income)</label>
            <input type="text" id="amount" v-model="transaction.amount" placeholder="Enter amount..." />
        </div>
        <button class="btn">Add transaction</button>
    </form>
</template>

<script setup>
import { reactive } from 'vue';
import { useToast } from 'vue-toastification';

const emit = defineEmits(['transactionSubmitted']);

const transaction = reactive({
    text: '',
    amount: 0
});

const toast = useToast();

const onSubmit = () => {
    if (!transaction.text || !transaction.amount) {
        toast.error('Both fields must be filled');
        return;
    }

    emit('transactionSubmitted', transaction);

    transaction.text = '';
    transaction.amount = 0;
};
</script>
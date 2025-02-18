<template>
    <Header />
    <div class="container">
        <Balance :balance="balance" />
        <incomeExpense :income="income" :expense="expense" />
        <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
        <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
    </div>
</template>

<script setup>
import Balance from './components/Balance.vue';
import Header from './components/Header.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import { ref, computed, onMounted } from 'vue';
import { useToast } from 'vue-toastification';

const toast = useToast();

onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

    if (savedTransactions) {
        transactions.value = savedTransactions;
    }
});

const balance = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0);
});

const transactions = ref([]);

const income = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount > 0)
        .reduce((acc, transaction) => {
            return acc + transaction.amount;
        }, 0)
        .toFixed(2);
});


const expense = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount < 0)
        .reduce((acc, transaction) => {
            return acc + transaction.amount;
        }, 0)
        .toFixed(2);
});

const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter((transaction) => transaction.id !== id);
    savedTransactionsToLocalStorage();
    toast.success('Transaction deleted')
}


const handleTransactionSubmitted = (transaction) => {
    transactions.value.unshift({
        id: generateUniqueId(),
        text: transaction.text,
        amount: parseFloat(transaction.amount)
    });

    savedTransactionsToLocalStorage();

    toast.success('Transaction Added!');
}


const generateUniqueId = () => {
    return Math.floor(Math.random() * 100000);
}


const savedTransactionsToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value));
}

</script>
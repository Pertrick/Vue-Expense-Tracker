<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <li v-for="transaction in transactions" :key="transaction.id" :class="transactionType(transaction)">
            {{ transaction.text }} <span>${{ transaction.amount }}</span>
            <button class="delete-btn" @click="deleteTransaction(transaction.id)">x</button>
        </li>
    </ul>
</template>



<script setup>
const emit = defineEmits(['transactionDeleted']);

const props = defineProps({
    'transactions': {
        type: Array,
        required: true
    }
});

const transactionType = function (transaction) {
    if (transaction.amount < 0) {
        return 'minus';
    } else {
        return 'plus'
    }
}

const deleteTransaction = (id) => {
    emit('transactionDeleted', id);
}

</script>
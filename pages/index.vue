<template>
    <Header />
    <div class="container">
        <Balance :total="total" />
        <IncomeExpenses :income="+income" :expenses="+expenses" />
        <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
        <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
    </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';

const transactions = ref([]);

onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

    if (savedTransactions) {
        transactions.value = savedTransactions;
    }
});

// Get total
const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0);
});

// Get income
const income = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount > 0)
        .reduce((acc, transaction) => acc + transaction.amount, 0)
        .toFixed(2);
});

// Get expenses
const expenses = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount < 0)
        .reduce((acc, transaction) => acc + transaction.amount, 0)
        .toFixed(2);
});

// Submit transaction
const handleTransactionSubmitted = (transactionData) => {
    transactions.value.push({
        id: generateUniqueId(),
        text: transactionData.text,
        amount: transactionData.amount,
    });

    saveTransactionsToLocalStorage();

    // Nuxt 3 doesn't have a built-in toast, so we'll need to add one later.
    // For now, we'll just log to the console.
    console.log('Transaction added.');
};

// Generate unique ID
const generateUniqueId = () => {
    return Math.floor(Math.random() * 1000000);
};

// Delete transaction
const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter(
        (transaction) => transaction.id !== id
    );

    saveTransactionsToLocalStorage();

    console.log('Transaction deleted.');
};

// Save transactions to local storage
const saveTransactionsToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value));
};
</script>
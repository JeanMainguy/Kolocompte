<template>
    <div :key="transaction.id" v-for="transaction in transactions">
        {{transaction.from}} doit à {{transaction.to}} {{transaction.amount}}€
    </div>
</template>


<script>

export default {
    name: 'Transactions', 
    
    props: {
        transactions: Array
    },
    
    components: {
        
    },


    methods: {
    }
    
}
</script>

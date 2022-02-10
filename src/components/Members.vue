<template>
    <div :key="member.id" v-for="member in members">
        <Expenses @update-member="updateMember" :member="member" />
    </div>
</template>


<script>
import Expenses from './Expenses'

export default {
    name: 'Members', 
    
    props: {
        members: Array
    },

    emits: ['update-member'],
    
    components: {
        Expenses
    },


    methods: {
        updateMember(member, value, operation) {
            //this.member.expenses = expenses
            console.log("NEW EXPENSE", value)

            this.$emit('update-member', member, value, operation)  
        },
    }
    
}
</script>

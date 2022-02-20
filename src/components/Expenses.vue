<template>
<div  class="member" >
  
        <li class=expense :key="expense" v-for="expense in member[type]">
        <h3>{{expense}} 
            <i @click="deleteValue(expense)" class="fas fa-times"></i>
        </h3>
        </li>
        <AddExpense @add-value="addValue" :label="type == 'food_expenses' ? 'Nourriture' : 'Charges'" />

</div>

</template>


<script >
import AddExpense from './AddExpense'

export default {
    name: 'Expenses', 
    props: {
        member: Object,
        type: String,

    },
    data() {
    return {
      expenses: [],
      expense_sum : Number,
    }
  },

  created() {
  },

    methods: {
        addValue(value) {
            
            console.log('THIS EXPENSES', this.expenses)
            this.$emit('update-member', this.member.id, value, "add", this.type)

        },

        deleteValue(value){
            this.$emit('update-member', this.member.id, value, "remove", this.type)

            // this.expenses = this.expenses.filter((exp) => exp !== expense)
            // this.expense_sum = this.expenses.reduce((a, b) => a + b, 0)
        }
    }, 
    components: {
        AddExpense
    }
}
</script>


<style scope>
.fas {
  color: rgb(207, 82, 82);
}
.member {
  background: #f5f5f5;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}
.expense {
    display: inline-block;
  background: #83d394;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}
.task.reminder {
  border-left: 5px solid green;
}
.task h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>

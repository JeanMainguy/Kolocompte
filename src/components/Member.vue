<template>
        <div class=member @dblclick="$emit('hide-all')" @click="$emit('expand-info', this.member.id)" >
            <h3> {{ this.member.name }} 


                    <!-- <i class="expense_sum">  <i class="fa-solid fa-calendar-exclamation"></i>{{ this.member.food_expense_sum }}€ </i>  -->


                     <i class="expense_sum fa-solid fa-pizza-slice"></i> {{ this.member.food_expense_sum }}€ 

                    <i class="expense_sum fa-solid fa-house"></i> {{ this.member.house_expense_sum }}€             

            <i class="expense_sum fa-solid fa-arrow-right-from-bracket"></i> {{this.member.week_away}}/4
            </h3>
            
        </div>
        <div v-if="this.member.showExpenses">
            <Expenses @update-member="updateMember" :member="this.member" type='house_expenses'/>
            <Expenses @update-member="updateMember" :member="this.member" type='food_expenses'/>
          <div>
        Absent
        <button class=btn @click="dec()">-</button>
        {{this.member.week_away}}
        <button class=btn @click="inc()">+</button> 
        semaines
        </div>

        </div>

      

</template>


<script>
import Expenses from './Expenses'

export default {
    name: 'Member', 
    
    props: {
        member: Object,
    },

    emits: ['update-member', 'hide-all', 'expand-info'],
    
    components: {
        Expenses
    },

    data() {
        return {
            showExpenses:false,
            
        }
    },


    methods: {
        updateMember(member, value, operation, expense_type) {
            //this.member.expenses = expenses
            console.log("NEW EXPENSE", value)

            this.$emit('update-member', member, value, operation, expense_type)  
        },
        inc() {
            this.$emit('update-member', this.member.id, 1, 'week_away')
        },
        dec() {
              this.$emit('update-member', this.member.id, -1, 'week_away')
            
        }
    },

}
</script>



<style scope>
.member {
  background: #f5f5f5;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}

.expense_sum {
    margin-left: 5%;
}
</style>
<template>

  <div class="center">
    <Header title="Kolocompte"/> 
    
  </div>
    <!-- <h3>Total : {{total_expenses}}</h3> -->

  <div class="container">
      <Header title="Dépenses"/>

      <div class=member :key="member.id" v-for="member in this.members">

         <Member @hide-all="hideAllMemberInfo" @expand-info="expandMemberInfo" @update-member="updateMember"  :member="member"/>
      </div>
      <div class="center">
      <!-- <Button @btn-click="solveBalance" text="Équilibres" color="green"/> -->
      
      </div>
  </div>
  <div class="container">
    <Header title="Balance"/>
    <Balance :members="members"/>

    <div class="center">
    </div>
</div>
  <div class="container">
    <Header title="Comment équilibrer?"/>
    <Transactions :transactions="transactions"/>

    <div class="center">
    </div>
</div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import Header from './components/Header.vue'
import Member from './components/Member.vue'
// import Button from "./components/Button.vue"
import Balance from "./components/Balance.vue"
import Transactions from "./components/Transactions.vue"

export default {
  name: 'App',
  components: {
     Header,
     Member,
    //  Button,
     Balance,
     Transactions
  }, 

  data() {
    return {
      members: [], 
      total_expenses: Number,
      total_weeks_spent:Number,
      total_food_expenses:Number,
      total_house_expenses:Number,
      sum_abs_balance:Number,
      transactions:[],
    }
  },
  
  methods: {
    updateAllMembers(){
      
      this.members = this.members.map( member => this.sumExpenses(member));


      this.total_house_expenses =  this.members.map(item => item.house_expense_sum).reduce((prev, next) => prev + next);
      
      this.total_food_expenses =  this.members.map(item => item.food_expense_sum).reduce((prev, next) => prev + next);
      
      this.total_expenses = this.total_house_expenses + this.total_food_expenses 

      this.total_weeks_spent = this.members.length*4 - this.members.map(item => item.week_away).reduce((prev, next) => prev + next);

      this.members = this.members.map( member => this.computeBalance(member));
      
      this.solveBalance()

    },

    sumExpenses(member){

      member.food_expense_sum = member.food_expenses.reduce((a, b) => a + b, 0);
      member.house_expense_sum = member.house_expenses.reduce((a, b) => a + b, 0);
      member.expense_sum = member.house_expenses.reduce((a, b) => a + b, 0);
      return member

    },
    computeBalance(member, key_name='balance'){


      let house_balance = member.house_expense_sum - (this.total_house_expenses/this.members.length)
      let food_balance =  member.food_expense_sum - (4 - member.week_away)*(this.total_food_expenses/this.total_weeks_spent)
      
      console.log("food per week and per personne", (this.total_food_expenses/this.total_weeks_spent))
      console.log(member.name, "is out ",member.week_away, "/4 weeks and for food need to pay", food_balance)

      member[key_name] = house_balance + food_balance
      
      return member

    },

    updateMember(member_index, value, operation, expense_type) {

      if (operation == "add") {
        this.members[member_index][expense_type] = [...this.members[member_index][expense_type], value]
      }

      else if (operation == "remove") {
        // this.members[member_index][expense_type] = this.members[member_index][expense_type].filter((exp) => exp !== value)
        const idx = this.members[member_index][expense_type].findIndex(v => v==value);
        this.members[member_index][expense_type].splice(idx,1);
      }
      else if (operation == "week_away") {
        let week_away =  this.members[member_index].week_away 
        week_away += value
        week_away = (week_away == -1 ? 4 : week_away)
        week_away %= 5

        this.members[member_index].week_away = Math.abs(week_away)

      }

      this.updateAllMembers()
      
    },

    roundWith2Decimal(num) {
      return Math.round((num + Number.EPSILON) * 100) / 100;

    },

    solveBalance() {

      
      this.transactions = [];

      this.members = this.members.map( member => this.computeBalance(member, "solved_balance"));
 
      let sorted_members = [...this.members ];

      let sum_abs_balance, creditor, debtor, amount;

      sum_abs_balance = sorted_members.map(item => item.solved_balance)
                  .reduce((prev, next) => Math.abs(prev) + Math.abs(next));

      console.log("INITIAL SUM BALAnCE IS", sum_abs_balance)
      let operation_count = 0
      
      while (this.roundWith2Decimal(sum_abs_balance) > 0 && operation_count < 25) {


        sorted_members =  sorted_members.sort((a, b) => (a.solved_balance > b.solved_balance ? -1 : 1))

        // console.log(sorted_members.map(x => x.name))

        creditor = sorted_members.at(0)
        debtor = sorted_members.at(-1)

        // console.log("the creditor is", creditor.name, creditor.solved_balance )

        // console.log("the debtor is", debtor.name, debtor.solved_balance)

        if (Math.abs(debtor.solved_balance) >= creditor.solved_balance){
            // console.log("debt >= credit")

  
            amount = creditor.solved_balance

            
            debtor.solved_balance =  debtor.solved_balance + creditor.solved_balance
            creditor.solved_balance = 0

        }
        else if (Math.abs(debtor.solved_balance) < creditor.solved_balance){
          // console.log("debt < credit")
          amount = Math.abs(debtor.solved_balance)
          
          creditor.solved_balance =  debtor.solved_balance  + creditor.solved_balance 

          debtor.solved_balance = 0

        }
        
        amount = this.roundWith2Decimal(amount)

        this.transactions = [...this.transactions, {from:debtor.name, 
                                                    to:creditor.name, 
                                                    amount:amount, 
                                                    id:operation_count}]

        console.log(operation_count,  debtor.name , "gives", amount, "to", creditor.name)

       sum_abs_balance = sorted_members.map(item => item.solved_balance)
                                      .reduce((prev, next) => Math.abs(prev) + Math.abs(next));

        
      // sorted_members.map(item => console.log(item.name, item.solved_balance, ))

      // console.log("SUM BALAnCE IS", sum_abs_balance, '--round it-->', this.roundWith2Decimal(sum_abs_balance))
      operation_count += 1;

      } 
      console.log("DONE RESOLVING... with ", operation_count, "operation")
    },

    hideAllMemberInfo(){
      console.log('HIDE ALL INFO')
      this.members = this.members.map(item => ({...item, showExpenses: false}))
      console.log(this.members)
      
    },

    expandMemberInfo (member_index) {

      console.log(this.members[member_index])
        this.members[member_index].showExpenses = !this.members[member_index].showExpenses
        },
        

 },

  created() {
    this.total_expenses = 0,

    this.total_weeks_spent = 0,

    this.members = [
      {
      id:0, 
      name:"Arthur",
      food_expenses:[],
     house_expenses:[],
      expense_sum:0,
      balance:0,
      week_away:0,
    }, 
    {
      id:1, 
      name:"Jean",
      food_expenses:[],
     house_expenses:[],
      expense_sum:0,
      balance:0,
      week_away:0,
    }, 
    {
      id:2,
      name:"Nao",
      food_expenses:[],
     house_expenses:[],
      expense_sum:0,
      balance:0,
      week_away:0,
    },
    {
      id:3,
      name:"Kiki",
      food_expenses:[],
     house_expenses:[],
      expense_sum:0,
      balance:0,
      week_away:0,
    },
    {
      id:4,
      name:"Clém",
      food_expenses:[],
     house_expenses:[],
      expense_sum:0,
      balance:0,
      week_away:0,
    },
    {
      id:5,
      name:"Simon",
      food_expenses:[],
     house_expenses:[],
      expense_sum:0,
      balance:0,
      week_away:0,
    } ]

  },


  mounted(){
    this.hideAllMemberInfo()
    this.updateAllMembers()


  },

}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Poppins', sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.center {
    display: flex;
    align-items: center;
    justify-content: center;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>

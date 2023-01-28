<template>
<div  class="member" >
  <li class=expense :key="name" v-for="name in member_names">
  <h3>{{name}}
      <i @click="deleteValue(name)" class="fas fa-times"></i>
  </h3>
  </li>

  <form @submit="addMember" class=add-form target="#here">
         <div class="form-control">
          <label>Nouveau coloc</label>
    
        <input class=member  name="name" 
               type="string"
               placeholder="prénom du coloc" 
               v-model="name" /><br />
    
        </div>
    </form>

</div>


    
    </template>
    
    
    <script>
    export default {
        name: 'AddMember',
    
        data() {
          return {
            name: "",
            }
        },
    
        props: {
          member_names: Array,
        },

        methods: {
        // addValue(value) {
            
        //     console.log('THIS EXPENSES', this.expenses)
        //     this.$emit('update-member', this.member.id, value, "add", this.type)

        // },
        addMember(e) {
            e.preventDefault()

            console.log("Add Name", this.name)

            this.$emit("add-member", String(this.name))
            
            this.name = ''
        
    },

        deleteValue(value){
            this.$emit('remove-member', value)

            // this.expenses = this.expenses.filter((exp) => exp !== expense)
            // this.expense_sum = this.expenses.reduce((a, b) => a + b, 0)
        }
    }, 
    
    }
    </script>
    
    
    <style scoped>
    .add-form {
      margin-bottom: 10px;
    }
    .form-control {
      margin: 10px 0;
    }
    .form-control label {
      display: block;
    }
    .form-control input {
      width: 50%;
      height: 40px;
      margin: 5px;
      padding: 3px 7px;
      font-size: 17px;
    }
    .form-control-check {
      display: flex;
      align-items: center;
      justify-content: space-between;
    }
    .form-control-check label {
      flex: 1;
    }
    .form-control-check input {
      flex: 2;
      height: 20px;
    }
    </style>
    
    
    
    
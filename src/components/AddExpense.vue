<template>

<form @submit="onSubmit" class=add-form target="#here">
     <div class="form-control">
      <label>Nouvelle dépense</label>

    <input class=member  name="expense_value" step="0.01" type="number" v-model="expense_value" /><br />

    </div>
</form>

</template>


<script>
export default {
    name: 'AddExpense',
      data() {
    return {
      expense_value: "",
    }
}, 
methods: {
    onSubmit(e) {
        e.preventDefault()

        this.expense_value


        console.log("VALU", this.expense_value)

        this.$emit("add-value", Number(this.expense_value))
        
        this.expense_value = ''
        
    }
}
}
</script>


<style scoped>
.add-form {
  margin-bottom: 40px;
}
.form-control {
  margin: 20px 0;
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




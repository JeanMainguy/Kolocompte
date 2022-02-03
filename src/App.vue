<template>
  <div class="container">
      <Header title="Kolocompte"/>
      <Members :members="members"/>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import Header from './components/Header.vue'
import Members from './components/Members.vue'

export default {
  name: 'App',
  components: {
     Header,
     Members, 
  }, 
  data() {
    return {
      members: []
    }
  },

  created() {
    this.members = [{
      id:1, 
      name:"Arthur",
      expenses:[],
      expense_sum:0,
      
    }, 
    {
      id:2,
      name:"Nao",
      expenses:[],
      expense_sum:0,
      
    },
    {
      id:3,
      name:"Kiki",
      expenses:[4,5,56],
      expense_sum:0,
      
    },
    {
      id:4,
      name:"Clémentine",
      expenses:[],
      expense_sum:0,
      
    },
    {
      id:5,
      name:"Simon",
      expenses:[],
      expense_sum:0,
      
    } ]

  }

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

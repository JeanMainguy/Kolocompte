<template>

    <div :key="member.id" v-for="member in members">
        <div  :class="[member.balance >= 0 ? 'positive' : 'negative', 'member']" >
            {{member.name}} {{roundWith2Decimal(member.balance)}}

        </div>
<!-- 
    <Button
        @btn-click="$emit('toggle-add-task')"
        text='Équilibre'
        color='green'
    /> -->
    </div>



</template>


<script>
// import Button from "./Button.vue"

export default {

    name: 'Balance', 
    
components: {
    //  Button,
  }, 


    props: {
        members: Array
    },
    methods: {
        roundWith2Decimal(num) {
            return Math.round((num + Number.EPSILON) * 100) / 100;


        },
    
    }
    
    
}



</script>

<style scope>

.member {
  background: #f5f5f5;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}

.member.positive {
  border-left: 5px solid green;
}

.member.negative {
  border-left: 5px solid red;
}

.task h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>


<template>
    <div :key="member.id" v-for="member in members">
        <Member :member="member" />
    </div>
</template>


<script>
import Member from './Member'

export default {
    name: 'Members', 
    
    props: {
        members: Array
    },
    components: {
        Member
    },
}
</script>

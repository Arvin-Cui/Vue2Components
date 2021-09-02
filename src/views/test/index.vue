<template>
  <div>
        <compA>

           <compB></compB>
        </compA>

  </div>
</template>

<script>
import compA from './compA.vue'
import compB from './compB.vue'
  export default {
    components:{
      compA,
      compB
    },
    methods: {
    },
  }
</script>

<style lang="scss" scoped>

</style>

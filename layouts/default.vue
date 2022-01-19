<template>
  <div class="container">
    <PageHeader @changeSorting="changeSorting"/>
   <NuxtChild :sortingValue="this.sortingValue"/>
  </div>
</template>

<script>
import PageHeader from '~/components/pageHeader.vue'
export default {
  data(){
    return{
      sortingValue: 'По умолчанию',
    }
  },
  components: {
    PageHeader,
  },
  methods: {
    changeSorting(value){
      this.sortingValue = value
    }
  },
}
</script>

<style scoped lang="scss">
.container {
  padding: 0 2.2%;
  display: flex;
  flex-direction: column;
  background-color: rgba(255, 254, 251, 0.8);
  // background-color: #dadada;
  height: 100vh;
}
</style>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>

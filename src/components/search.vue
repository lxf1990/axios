<template>
  <div>
    <input type="text" placeholder="enter you want search name" v-model="inputSearchName">
    <button @click="search">搜索Github用户</button>
  </div>
</template>
<script>
    export default {
        props:['setSearchName'],
        data () {
           return{
             inputSearchName:''
           }
        },
      methods:{
            search(){
              var inputSearchName = this.inputSearchName.trim()
              if(!inputSearchName){
                    return
              }
              this.setSearchName(inputSearchName)
            }
      }


    }
</script>
<style>

</style>

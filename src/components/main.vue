<template>
  <div>
    <!--//第一次进入时的状态-->
    <h2 v-show="firstView">Enter name to search</h2>
    <!--//加载时的状态-->
    <h2 v-show="loading">loading......</h2>
    <!--//错误时的状态-->
    <h2 v-show="errorMsg">{{errorMsg}}</h2>

    <!--//正常时的显示状态-->
    <div class="row" v-show="users">
       <div class="card" v-for="user in users">
         <a :href="user.html_url" target="_blank">
           <img :src="user.avatar_url" style="width:100px">
         </a>
         <p class="card-text">{{user.login}}</p>
       </div>
    </div>

  </div>



</template>
<script>
  import axios from 'axios'
    export default {
        props:['searchName'],
      data () {
        return{
          firstView:true,
          loading:false,
          errorMsg:null,
          users:null
        }

      },
      watch:{
            searchName(newValue,oldValue){
                //然后开始更新状态
              this.firstView =false
              this.loading = true
              this.errorMsg = null
              this.users = null

              var url = `https://api.github.com/search/users?q=${this.searchName}`

              axios.get(url)
                .then(response =>{
                    var result = response.data
                    this.loading = false
                  this.users = result.items.map(item =>{
                      return {
                          html_url:item.html_url,
                        avatar_url:item.avatar_url,
                         login:item.login
                      }
                  })
                })
                .catch(error =>{
                    this.loading = false
                  this.errorMsg = '请求失败了'
                })

            }
      }
    }
</script>
<style>
  .card {
    float: left;
    width: 33.333%;
    padding: .75rem;
    margin-bottom: 2rem;
    border: 1px solid #efefef;
    text-align: center;
  }

  .card > img {
    margin-bottom: .75rem;
    border-radius: 100px;
  }

  .card-text {
    font-size: 85%;
  }
</style>

<template>
   <div class="col-sm-9 col-sm-offset-3 col-md-10 col-md-offset-2 main">
            <h2 class="sub-header">武器列表</h2>
            <!-- <a class="btn btn-success" href="/weapon/add">Add</a> -->
            <router-link class="btn btn-success" :to="{name: 'weaponadd'}">添加</router-link>
            <div class="table-responsive">
              <table class="table table-striped">
                <thead>
                  <tr>
                    <th>#</th>
                    <th>编号</th>
                    <th>名称</th>
                    <th>能量</th>
                    <th>操作</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(item,index) in list" :key="item.id">
                    <td>{{index+1}}</td>
                    <td>{{item.id}}</td>
                    <td>{{item.name}}</td>
                    <td>{{item.power}}</td>
                    <td>
                      <!-- router-link 路由变化的时候用 -->
                      <router-link :to="{name: 'weaponedit',params:{id:item.id}}">修改</router-link>
                      &nbsp;&nbsp;
                      <a href="javascript:;" @click="del(item.id)">删除</a>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>  
</template>

<script>
// import axios from 'axios'
export default {
  data(){
    return {
      list:[]
    }
  },
  mounted(){
    this.loadData()
  },
  methods:{
    //列表显示
    loadData(){
      this.axios
      .get('weapons')
      .then((response) => {
        //解构
        const {data,status} = response;
        if(status === 200){
          this.list = data
        }else {
          alert("获取数据失败")
        }
      })
      .catch((err) =>{
          alert('服务器错误'+err)

      })
    },
    //删除
    del(id){
      if(!confirm('确定要删除?')){
        return false;
      }
      this.axios
      .delete(`weapons/${id}`)
      .then((response) => {
        const status = response.status;
        if(status === 200){
          this.loadData();
          alert('删除成功')
        }else{
          alert('删除失败')
        }
      })
      .catch((err) => {
        
        alert('服务器错误--' + err);
      })
    }
  }
}
</script>

<style>

</style>

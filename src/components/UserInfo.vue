<template>
  <div class="Info">
    <h1>用户信息管理系统</h1>
    <el-row>
    	<el-col :span="20" :push="2">
        <div>
          <el-form :inline="true">

            <el-form-item style="float: left" label="查询用户信息">
              <el-input v-model="keyUser" placeholder="查询所需要的内容........."></el-input>
            </el-form-item>

            <el-form-item style="float: right">
              <el-button type="primary" size="small" icon="el-icon-edit-outline" @click="addFromUserInfo">添加</el-button>
            </el-form-item>
          </el-form>


        </div>
        <div>
    		 <el-table
          :data="searchUserinfo(keyUser)"
          style="width: 100%">
          <el-table-column
            type="index"
            label="序号"
            width="90">
          </el-table-column>
          <el-table-column
            label="日期"
            width="120">
            <template slot-scope="scope">
              <i class="el-icon-time"></i>
              <span style="margin-left: 10px">{{ scope.row.date | moment}}</span>
            </template>
          </el-table-column>
          <el-table-column
            label="姓名"
            width="100">
            <template slot-scope="scope">
              <span style="margin-left: 10px">{{ scope.row.name }}</span>
            </template>
          </el-table-column>
          <el-table-column
            label="邮箱"
            width="160">
            <template slot-scope="scope">
              <span style="margin-left: 10px">{{ scope.row.email }}</span>
            </template>
          </el-table-column>
          <el-table-column
            label="标题"
            width="160">
            <template slot-scope="scope">
              <span style="margin-left: 10px">{{ scope.row.title }}</span>
            </template>
          </el-table-column><el-table-column
            label="评价"
            width="160">
            <template slot-scope="scope">
              <span style="margin-left: 10px">{{ scope.row.evaluate }}</span>
            </template>
          </el-table-column>
          <el-table-column
            label="状态"
            width="160">
            <template slot-scope="scope">
                 <span style="margin-left: 10px">{{ scope.row.state }}</span>
            </template>
          </el-table-column>
          <el-table-column label="操作">
            <template slot-scope="scope">
              <el-button
                size="mini"
                @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
              <el-button
                size="mini"
                type="danger"
                @click="handleDelete(scope.$index, scope.row)">删除</el-button>
            </template>
          </el-table-column>
        </el-table>
      </div>
    	</el-col>
    </el-row>
    <AddInfo :addfrom="addfrom" @addUpdate="UserInfo"></AddInfo>
    <EditInfo :editfrom="editfrom" :fromEdit="fromEdit" @editUpdate="UserInfo"></EditInfo>
  </div>
</template>

<script>
  import AddInfo from './AddInfo.vue'
  import EditInfo from './EditInfo.vue'
export default {
  name: 'UserInfo',
  data () {
    return {
      tableData:[],
      addfrom:{
        show:false
      },
      editfrom:{
        show:false
      },
      fromEdit:{
        date:"",
        name:"",
        email:"",
        title:"",
        evaluate:"",
        state:""
      },
      keyUser:""
    }
  },
  methods:{
    UserInfo(){
      this.$axios.get('http://localhost:3000/data').then(res =>{
        this.tableData=res.data;
        console.log(this.tableData)
      })
    },
    addFromUserInfo(){
      this.addfrom.show=true
    },
    handleEdit(index,row){
      this.editfrom.show=true;
      this.fromEdit = {
        date: row.date,
        name:row.name,
        email:row.email,
        title:row.title,
        evaluate:row.evaluate,
        state:row.state,
        id:row.id
      }
    },
    handleDelete(indx,row){
    this.$axios.delete(`http://localhost:3000/data/${row.id}`).then(row=>{
      this.$message({
        type:"success",
        message:"删除成功",
      });
      this.UserInfo();
    })
  },
  searchUserinfo(keyUser){
    return this.tableData.filter(user =>{
      if(user.name.includes(keyUser)){
        return user
      }
    })
  }
  },
  created(){
    this.UserInfo();
  },
  components:{
    AddInfo,
    EditInfo
  }
  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1{
	font-size: 30px;
	text-align: center;
	color: #333;
	width: 300px;
	margin:  0 auto;
	border-bottom: 2px solid red ;
	margin-bottom: 30px; 
}
</style>

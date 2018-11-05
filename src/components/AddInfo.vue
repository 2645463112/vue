<template>
  <div class="add">
    <el-dialog title="添加用户信息" :visible.sync="addfrom.show">
	  <el-form :model="form" label-width="100px" :rules="rules" ref="addinfo">
	    <el-form-item label="日期" prop="date">
			<el-date-picker
		      v-model="form.date"
		      type="date"
		      placeholder="选择日期">
		    </el-date-picker>
	    </el-form-item>
	    <el-form-item label="姓名" prop="name">
	      <el-input v-model="form.name">
		    </el-input>
	      </el-form-item>
		  <el-form-item label="邮箱"  prop="email">
		      <el-input v-model="form.email">
		    </el-input>
		  </el-form-item>

		  <el-form-item label="标题">
		      <el-input v-model="form.title">
		    </el-input>
		  </el-form-item>

		  <el-form-item label="评价">
		      <el-input v-model="form.evaluate">
		    </el-input>
		  </el-form-item>

		  <el-form-item label="状态">
		      <el-input v-model="form.state">
		    </el-input>
	  </el-form-item>
	  </el-form>

	  
	  <div slot="footer" class="dialog-footer">
	    <el-button @click="addfrom.show = false">取 消</el-button>
	    <el-button type="primary" @click="addinFrom('addinfo')">确 定</el-button>
	  </div>
	</el-dialog>
  </div>
</template>

<script>
export default {
  name: 'AddInfo',
  data () {
    return{
    	form:{
    		date:"",
    		name:"",
    		email:"",
    		title:"",
    		evaluate:"",
    		state:""
    	},
    	rules:{
    		date:[{required:true,message:"日期不能为空"}],
    		name:[{required:true,message:"用户名不能为空"}],
    		email:[{required:true,message:"邮箱不能为空"}],
    	}
    }

    },
    methods:{
    	addinFrom(addinfo){
this.$refs[addinfo].validate((valid) => {
          if (valid) {
            this.$axios.post('http://localhost:3000/data',this.form).then(res => {
            	console.log(res)
            	this.$message({
            		type:"success",
            		message:"添加信息成功"
            	}),
            	this.addfrom.show = false; 

            	this.$emit('addUpdate');
            })
          } else {
            console.log('error submit!!');
            return false;
          }
        });
  	}
  },
    props:{
    	addfrom:Object
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>

<template>
  <div class="add">
    <el-dialog title="添加用户信息" :visible.sync="editfrom.show">
	  <el-form :model="fromEdit" label-width="100px" :rules="rules" ref="editinfo">
	    <el-form-item label="日期" prop="date">
			<el-date-picker
		      v-model="fromEdit.date"
		      type="date"
		      placeholder="选择日期">
		    </el-date-picker>
	    </el-form-item>
	    <el-form-item label="姓名" prop="name">
	      <el-input v-model="fromEdit.name">
		    </el-input>
	      </el-form-item>
		  <el-form-item label="邮箱"  prop="email">
		      <el-input v-model="fromEdit.email">
		    </el-input>
		  </el-form-item>

		  <el-form-item label="标题">
		      <el-input v-model="fromEdit.title">
		    </el-input>
		  </el-form-item>

		  <el-form-item label="评价">
		      <el-input v-model="fromEdit.evaluate">
		    </el-input>
		  </el-form-item>

		  <el-form-item label="状态">
		      <el-input v-model="fromEdit.state">
		    </el-input>
	  </el-form-item>
	  </el-form>

	  
	  <div slot="footer" class="dialog-footer">
	    <el-button @click="editfrom.show = false">取 消</el-button>
	    <el-button type="primary" @click="editInfoFrom('editinfo')">确 定</el-button>
	  </div>
	</el-dialog>
  </div>
</template>

<script>
export default {
  name: 'AddInfo',
   props:{
      editfrom:Object,
      fromEdit:Object
    },
  data () {
    return{
    	rules:{
    		date:[{required:true,message:"日期不能为空"}],
    		name:[{required:true,message:"用户名不能为空"}],
    		email:[{required:true,message:"邮箱不能为空"}],
    	}
    }
  },
  methods:{
    editInfoFrom(editinfo){
      this.$refs[editinfo].validate((valid) => {
          if (valid) {
            this.$axios.put(`http://localhost:3000/data/${this.fromEdit.id}`,this.fromEdit).then(res => {
              console.log(res)
              this.$message({
                type:"success",
                message:"添加信息成功"
              }),
              this.editfrom.show = false; 

              this.$emit('editUpdate');
            })
          } else {
            console.log('error submit!!');
            return false;
          }
        });
    }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>

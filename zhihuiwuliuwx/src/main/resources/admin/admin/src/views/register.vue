<template>
    <div>
        <div class="container">
            <div class="login-form">
                <h1 class="h1" style="color:#000;fontSize:28px;">智慧物流小程序注册</h1>
                <el-form class="rgs-form">
                    <el-form-item label="账号" class="input">
                        <el-input v-model="ruleForm.username" autocomplete="off" placeholder="账号"  />
                    </el-form-item>
                    <el-form-item label="密码" class="input">
                        <el-input type="password" v-model="ruleForm.password" autocomplete="off" show-password/>
                    </el-form-item>
                    <el-form-item label="重复密码" class="input">
                        <el-input type="passwo   rd" v-model="ruleForm.repetitionPassword" autocomplete="off" show-password/>
                    </el-form-item>
                        <el-form-item label="司机名称" class="input" v-if="tableName=='siji'">
                            <el-input v-model="ruleForm.sijiName" autocomplete="off" placeholder="司机名称"  />
                        </el-form-item>
                        <el-form-item label="联系方式" class="input" v-if="tableName=='siji'">
                            <el-input v-model="ruleForm.sijiPhone" autocomplete="off" placeholder="联系方式"  />
                        </el-form-item>
                        <el-form-item label="邮箱" class="input" v-if="tableName=='siji'">
                            <el-input v-model="ruleForm.sijiEmail" autocomplete="off" placeholder="邮箱"  />
                        </el-form-item>
                        <!--
                            <el-form-item v-if="sijiPhotoFlag">
                            </el-form-item>
                            <el-form-item label="司机头像" class="input" v-if="tableName=='siji'">
                                    <file-upload
                                        tip="点击上传司机头像"
                                        action="file/upload"
                                        :limit="1"
                                        :multiple="true"
                                        :fileUrls="ruleForm.sijiPhoto?$base.url+ruleForm.sijiPhoto:''"
                                        @change="sijiPhotoUploadChange"
                                    ></file-upload>
                           </el-form-item>
                        -->
                       <!--<el-form-item class="input" label="司机类型">
                            <el-select style="width: 399px;height: 40px;" v-model="ruleForm.sijiTypes" placeholder="请选择司机类型">
                                <el-option
                                        v-for="(item,index) in sijiTypesOptions"
                                        v-bind:key="item.codeIndex"
                                        :label="item.indexName"
                                        :value="item.codeIndex">
                                </el-option>
                            </el-select>
                        </el-form-item>-->
                        <!--
                            <el-form-item label="司机介绍" class="input" v-if="tableName=='siji'">
                                <el-input type="textarea" v-model="ruleForm.sijiContent" placeholder="司机介绍"></el-input>
                           </el-form-item>
                        -->
                        <!--
                            <el-form-item label="逻辑删除" class="input" v-if="tableName=='siji'">
                                <el-input v-model="ruleForm.dataDelete" autocomplete="off" placeholder="逻辑删除"  />
                           </el-form-item>
                        -->
                        <!--
                            <el-form-item label="创建时间" class="input" v-if="tableName=='siji'">
                                <el-input v-model="ruleForm.createTime" autocomplete="off" placeholder="创建时间"  />
                           </el-form-item>
                        -->
                        <el-form-item label="用户姓名" class="input" v-if="tableName=='yonghu'">
                            <el-input v-model="ruleForm.yonghuName" autocomplete="off" placeholder="用户姓名"  />
                        </el-form-item>
                        <el-form-item label="联系方式" class="input" v-if="tableName=='yonghu'">
                            <el-input v-model="ruleForm.yonghuPhone" autocomplete="off" placeholder="联系方式"  />
                        </el-form-item>
                        <el-form-item label="身份证号" class="input" v-if="tableName=='yonghu'">
                            <el-input v-model="ruleForm.yonghuIdNumber" autocomplete="off" placeholder="身份证号"  />
                        </el-form-item>
                        <!--
                            <el-form-item v-if="yonghuPhotoFlag">
                            </el-form-item>
                            <el-form-item label="用户头像" class="input" v-if="tableName=='yonghu'">
                                    <file-upload
                                        tip="点击上传用户头像"
                                        action="file/upload"
                                        :limit="1"
                                        :multiple="true"
                                        :fileUrls="ruleForm.yonghuPhoto?$base.url+ruleForm.yonghuPhoto:''"
                                        @change="yonghuPhotoUploadChange"
                                    ></file-upload>
                           </el-form-item>
                        -->
                        <el-form-item label="电子邮箱" class="input" v-if="tableName=='yonghu'">
                            <el-input v-model="ruleForm.yonghuEmail" autocomplete="off" placeholder="电子邮箱"  />
                        </el-form-item>
                        <!--
                            <el-form-item label="余额" class="input" v-if="tableName=='yonghu'">
                                <el-input v-model="ruleForm.newMoney" autocomplete="off" placeholder="余额"  />
                           </el-form-item>
                        -->
                        <!--
                            <el-form-item label="逻辑删除" class="input" v-if="tableName=='yonghu'">
                                <el-input v-model="ruleForm.dataDelete" autocomplete="off" placeholder="逻辑删除"  />
                           </el-form-item>
                        -->
                        <!--
                            <el-form-item label="创建时间" class="input" v-if="tableName=='yonghu'">
                                <el-input v-model="ruleForm.createTime" autocomplete="off" placeholder="创建时间"  />
                           </el-form-item>
                        -->
                    <div style="display: flex;flex-wrap: wrap;width: 100%;justify-content: center;">
                        <el-button class="btn" type="primary" @click="login()">注册</el-button>
                        <el-button class="btn close" type="primary" @click="close()">取消</el-button>
                    </div>
                </el-form>
            </div>
        </div>
    </div>
</template>
<script>
    export default {
        data() {
            return {
                ruleForm: {
                },
                tableName:"",
                rules: {},
                sexTypesOptions : [],
                sijiPhotoFlag:false,//用于刷新文件上传
                yonghuPhotoFlag:false,//用于刷新文件上传
            };
        },
        mounted(){
            let table = this.$storage.get("loginTable");
            this.tableName = table;

            //级联表的下拉框查询

        },
        methods: {
            // 获取uuid
            getUUID () {
                return new Date().getTime();
            },
            sijiPhotoUploadChange(fileUrls) {
               this.ruleForm.sijiPhoto = fileUrls;
                if(this.sijiPhotoFlag){
                    this.sijiPhotoFlag=false;
                }else{
                    this.sijiPhotoFlag=true;
                }
            },
            yonghuPhotoUploadChange(fileUrls) {
               this.ruleForm.yonghuPhoto = fileUrls;
                if(this.yonghuPhotoFlag){
                    this.yonghuPhotoFlag=false;
                }else{
                    this.yonghuPhotoFlag=true;
                }
            },
            close(){
                this.$router.push({ path: "/login" });
            },
            // 注册
            login() {

                            if((!this.ruleForm.username)){
                                this.$message.error('账号不能为空');
                                return
                            }
                            if((!this.ruleForm.password)){
                                this.$message.error('密码不能为空');
                                return
                            }
                            if((!this.ruleForm.repetitionPassword)){
                                this.$message.error('重复密码不能为空');
                                return
                            }
                            if(this.ruleForm.repetitionPassword != this.ruleForm.password){
                                this.$message.error('密码和重复密码不一致');
                                return
                            }
                            if((!this.ruleForm.sijiName)&& 'siji'==this.tableName){
                                this.$message.error('司机名称不能为空');
                                return
                            }
                             if('siji' == this.tableName && this.ruleForm.sijiPhone&&(!this.$validate.isMobile(this.ruleForm.sijiPhone))){
                                 this.$message.error('手机应输入手机格式');
                                 return
                             }
                            if('siji' == this.tableName && this.ruleForm.sijiEmail&&(!this.$validate.isEmail(this.ruleForm.sijiEmail))){
                                this.$message.error("邮箱应输入邮件格式");
                                return
                            }
                            // siji 中的 司机头像 判空处理
                            // if('siji' == this.tableName && (this.ruleForm.sijiPhoto== null ||this.ruleForm.sijiPhoto== "")){
                            //     this.$message.error("司机头像不能为空");
                            //     return
                            // }
                            // siji 中的 司机类型 判空处理
                            // if('siji' == this.tableName && (this.ruleForm.sijiTypes== null ||this.ruleForm.sijiTypes== "")){
                            //     this.$message.error("司机类型不能为空");
                            //     return
                            // }
                            // siji 中的 司机介绍 判空处理
                            // if('siji' == this.tableName && (this.ruleForm.sijiContent== null ||this.ruleForm.sijiContent== "")){
                            //     this.$message.error("司机介绍不能为空");
                            //     return
                            // }
                            // siji 中的 逻辑删除 判空处理
                            // if('siji' == this.tableName && (this.ruleForm.dataDelete== null ||this.ruleForm.dataDelete== "")){
                            //     this.$message.error("逻辑删除不能为空");
                            //     return
                            // }
                            // siji 中的 创建时间 判空处理
                            // if('siji' == this.tableName && (this.ruleForm.createTime== null ||this.ruleForm.createTime== "")){
                            //     this.$message.error("创建时间不能为空");
                            //     return
                            // }
                            if((!this.ruleForm.yonghuName)&& 'yonghu'==this.tableName){
                                this.$message.error('用户姓名不能为空');
                                return
                            }
                             if('yonghu' == this.tableName && this.ruleForm.yonghuPhone&&(!this.$validate.isMobile(this.ruleForm.yonghuPhone))){
                                 this.$message.error('手机应输入手机格式');
                                 return
                             }
                            if((!this.ruleForm.yonghuIdNumber)&& 'yonghu'==this.tableName){
                                this.$message.error('身份证号不能为空');
                                return
                            }
                            // yonghu 中的 用户头像 判空处理
                            // if('yonghu' == this.tableName && (this.ruleForm.yonghuPhoto== null ||this.ruleForm.yonghuPhoto== "")){
                            //     this.$message.error("用户头像不能为空");
                            //     return
                            // }
                            if('yonghu' == this.tableName && this.ruleForm.yonghuEmail&&(!this.$validate.isEmail(this.ruleForm.yonghuEmail))){
                                this.$message.error("邮箱应输入邮件格式");
                                return
                            }
                            // yonghu 中的 余额 判空处理
                            // if('yonghu' == this.tableName && (this.ruleForm.newMoney== null ||this.ruleForm.newMoney== "")){
                            //     this.$message.error("余额不能为空");
                            //     return
                            // }
                            // yonghu 中的 逻辑删除 判空处理
                            // if('yonghu' == this.tableName && (this.ruleForm.dataDelete== null ||this.ruleForm.dataDelete== "")){
                            //     this.$message.error("逻辑删除不能为空");
                            //     return
                            // }
                            // yonghu 中的 创建时间 判空处理
                            // if('yonghu' == this.tableName && (this.ruleForm.createTime== null ||this.ruleForm.createTime== "")){
                            //     this.$message.error("创建时间不能为空");
                            //     return
                            // }
                this.$http({
                    url: `${this.tableName}/register`,
                    method: "post",
                    data:this.ruleForm
                }).then(({ data }) => {
                    if (data && data.code === 0) {
                    this.$message({
                        message: "注册成功,请登录后在个人中心页面补充个人数据",
                        type: "success",
                        duration: 1500,
                        onClose: () => {
                        this.$router.replace({ path: "/login" });
                }
                });
                } else {
                    this.$message.error(data.msg);
                }
            });
            }
        }
    };
</script>
<style lang="scss" scoped>
	.el-radio__input.is-checked .el-radio__inner {
		border-color: #00c292;
		background: #00c292;
	}

	.el-radio__input.is-checked .el-radio__inner {
		border-color: #00c292;
		background: #00c292;
	}

	.el-radio__input.is-checked .el-radio__inner {
		border-color: #00c292;
		background: #00c292;
	}

	.el-radio__input.is-checked+.el-radio__label {
		color: #00c292;
	}

	.el-radio__input.is-checked+.el-radio__label {
		color: #00c292;
	}

	.el-radio__input.is-checked+.el-radio__label {
		color: #00c292;
	}

	.h1 {
		margin-top: 10px;
	}

	body {
		padding: 0;
		margin: 0;
	}

	.nk-navigation {
		margin-top: 15px;

		a {
			display: inline-block;
			color: #fff;
			background: rgba(255, 255, 255, .2);
			width: 100px;
			height: 50px;
			border-radius: 30px;
			text-align: center;
			display: flex;
			align-items: center;
			margin: 0 auto;
			justify-content: center;
			padding: 0 20px;
		}

		.icon {
			margin-left: 10px;
			width: 30px;
			height: 30px;
		}
	}

	.register-container {
		margin-top: 10px;

		a {
			display: inline-block;
			color: #fff;
			max-width: 500px;
			height: 50px;
			border-radius: 30px;
			text-align: center;
			display: flex;
			align-items: center;
			margin: 0 auto;
			justify-content: center;
			padding: 0 20px;

			div {
				margin-left: 10px;
			}
		}
	}

	.container {
		height: 100vh;
		background-position: center center;
		background-size: cover;
		background-repeat: no-repeat;
		background-image: url(/zhihuiwuliuwx/img/back-img-bg.jpg);

		.login-form {
			right: 50%;
			top: 50%;
			transform: translate3d(50%, -50%, 0);
			border-radius: 10px;
			background-color: rgba(255,255,255,.5);
			font-size: 14px;
			font-weight: 500;
      box-sizing: border-box;

			width: 450px;
			height: auto;
			padding: 0;
			margin: 0;
			border-radius: 50px;
			border-width: 3px;
			border-style: solid;
			border-color: var(--publicMainColor);
			background-color: rgba(255, 255, 255, 0.9);
			box-shadow: 0 0 0px 15px rgba(255, 255, 255, 1);

			.h1 {
				width: 400px;
				height: 80px;
				line-height:80px;
				color: #000;
				font-size: 18px;
				padding: 0;
				margin: 10px 0px 20px 10px;
				border-radius: 0;
				border-width: 0;
				border-style: solid;
				border-color: rgba(237, 237, 237, 1);
				background-color: rgba(255, 69, 0, 0);
				box-shadow: 0 0 6px rgba(255,0,0,0);
				text-align: center;
			}

			.rgs-form {
				display: flex;
				flex-direction: column;
				justify-content: center;
				align-items: center;

        .el-form-item {
          width: 100%;
          display: flex;

          & /deep/ .el-form-item__content {
            flex: 1;
            display: flex;
          }
        }

				.input {
          width: 360px;
          height:40px;
          padding: 0;
          margin: 0px 0px 10px 50px;
          border-radius: 0;
          border-width: 0;
          border-style: solid;
          border-color: rgba(255,0,0,0);
          background-color: rgba(144, 238, 144, 0);
          box-shadow: 0 0 6px rgba(255,0,0,0);

					& /deep/ .el-form-item__label {
            width: 80px;
            line-height:40px;
            color: rgba(0, 0, 0, 1);
            font-size: 16px;
            padding: 0px 0px 0px 0px;
            margin: 0px 0px 0px -15px;
            border-radius: 0;
            border-width: 0;
            border-style: solid;
            border-color: rgba(255,0,0,0);
            background-color: rgba(144, 238, 144, 0);
            box-shadow: 0 0 6px rgba(255,0,0,0);
					}

					& /deep/ .el-input__inner {
            width: 200px;
            height: 40px;
            line-height:40px;
            color: var(--publicSubColor);
            font-size: 14px;
            padding: 0px  0px 0px 10px;
            margin: 0px 0px 0px 10px;
            border-radius: 20px;
            border-width: 3px;
            border-style: solid;
            border-color: var(--publicSubColor);
            background-color: rgba(255, 255, 255, 1);
            box-shadow: 0 0 6px rgba(255,0,0,0);
            text-align: left;
					}
				}

        .send-code {
          & /deep/ .el-input__inner {
            width: 120px;
            height: 40px;
            line-height:40px;
            color: #fff;
            font-size: 14px;
            padding: 0px 12px 0px 0px;
            margin: 0px 0px 0px 10px;
            border-radius: 20px 0px 0px 20px;
            border-width: 3px;
            border-style: solid;
            border-color: var(--publicMainColor);
            background-color: rgba(255, 255, 255, 0.8);
            box-shadow: 0 0 6px rgba(255,0,0,0);
            text-align: left;
          }

          .register-code {
            margin: 0px 0px 0px 0px;
            padding: 5px;
            width: 80px;
            height: 40px;
            line-height:40px;
            color: #fff;
            font-size: 14px;
            border-width: 0;
            border-style: solid;
            border-color: rgba(255,0,0,0);
            border-radius: 0 20px 20px 0;
            background-color: rgba(96, 98, 102, 1);
            box-shadow: 0 0 6px rgba(255,0,0,0);
          }
        }

				.btn {
					margin: 10px 60px 40px 0px ;
          padding: 0;
					width: 88px;
					height: 32px;
          line-height:32px;
					color: #fff;
					font-size: 14px;
					border-width: 0;
					border-style: solid;
					border-color: #409EFF;
					border-radius: 20px;
					background-color: var(--publicSubColor);
          box-shadow: 0 0 0px 4px rgba(255,255,255,1);
				}

				.close {
          margin: 10px 20px 40px 0px;
          padding: 0;
          width: 88px;
          height: 32px;
          line-height:32px;
          color: rgba(255, 255, 255, 1);
          font-size: 14px;
          border-width: 0;
          border-style: solid;
          border-color: #409EFF;
          border-radius: 20px;
          background-color: var(--publicMainColor);
          box-shadow: 0 0 0px 5px rgba(255,255,255,1);
				}

			}
		}
	}
</style>



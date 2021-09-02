<template>
	<div>
		<el-form :rules="rules" ref="Form" :model="Form">
			<el-form-item label="姓名" prop="name">
				<Input v-model="Form.name" placeholder="请输入姓名" />
			</el-form-item>
			<el-form-item label="密码" prop="pwd">
				<Input v-model="Form.pwd" placeholder="请输入密码" />
			</el-form-item>
			<el-form-item>
				<el-button
					type="primary"
					@click="submitForm"
					style="marginLeft:110px ;"
					>登录</el-button
				>
			</el-form-item>
		</el-form>
	</div>
</template>

<script>
	import ElForm from "./ElForm.vue";
	import ElFormItem from "./ElFormItem.vue";
	import Input from "./Input.vue";
	export default {
		components: {
			ElForm,
			ElFormItem,
			Input
		},
		data() {
			return {
				Form: { name: "", pwd: "" },
				rules: {
					name: [
						{ required: true, message: "用户名不能为空" },
						{ min: 8, message: "用户名必须大于8个字符" },
						{ max: 16, message: "用户名必须小于16个字符" }
					],
					pwd: [
						{ required: true, message: "密码不能为空" },
						{ min: 8, message: "密码必须大于8个字符" },
						{ max: 16, message: "密码必须小于16个字符" }
					]
				}
			};
		},
		methods: {
			submitForm() {
				this.$refs.Form.validate(ret => {
					if (ret === false) {
						return false;
					} else {
						this.$message.success("校验通过");
					}
				});
			}
		}
	};
</script>

<style lang="scss" scoped>
</style>

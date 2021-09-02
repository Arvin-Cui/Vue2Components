<template>
	<el-card class="box-card">
		<div>
			<slot></slot>
		</div>
	</el-card>
</template>

<script>
	export default {
		name: "ElForm",
		provide() {
			return {
				form: this
			};
		},
		created() {
			this.formItemList = [];
		},
		props: {
			model: {
				type: Object,
				required: true
			},
			rules: {
				type: Object
			}
		},
		methods: {
			formItemAdd(formItem) {
				this.formItemList.push(formItem);
			},
			async validate(callback) {
				const res = this.formItemList.map(item => {
					return item.doValidate();
				});
				console.log(res);
				const results = await Promise.all(res);
				let ret = true;
				results.map(result => {
					if (!result) {
						ret = false;
					}
				});
				callback(ret);
			}
		}
	};
</script>

<style lang="scss" scoped>
	.box-card {
		margin: 10px;
	}
</style>

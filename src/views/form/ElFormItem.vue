<template>
	<div>
		<div class="formItem">
			<span v-if="label" class="label">{{ label }}:</span>
			<slot></slot>
		</div>
		<div v-show="errorMessage" class="error">
			{{ errorMessage }}
		</div>
	</div>
</template>

<script>
	import Schema from "async-validator";
	export default {
		props: {
			label: {
				type: String,
				default: ""
			},
			prop: {
				type: String
			}
		},
		inject: ["form"],
		mounted() {
			this.$on("doValidate", this.doValidate);
			if (this.prop) {
				this.form.formItemAdd(this);
			}
		},
		data() {
			return {
				errorMessage: ""
			};
		},
		methods: {
			doValidate() {
				return new Promise((resolve, reject) => {
					const des = { [this.prop]: this.form.rules[this.prop] };
					const validate = new Schema(des);
					validate.validate(
						{ [this.prop]: this.form.model[this.prop] },
						err => {
							if (!err) {
								this.errorMessage = "";
								resolve(true);
							} else {
								this.errorMessage = err[0].message;
								resolve(false);
							}
						}
					);
				});
			}
		}
	};
</script>

<style lang="scss" scoped>
	.box {
		display: flex;
	}
	.formItem {
		display: flex;
		align-items: center;
		margin: 10px;
	}
	.label {
		text-align: center;
		width: 100px;
	}
	.error {
		margin-left: 130px;
		color: #ff0000;
	}
</style>

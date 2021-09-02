<template>
	<div class="box">
		<div class="imgbox">
			<img :src="src" alt="" @click="handleview" />
		</div>
		<transition name="el-fade-in-linear">
			<div
				class="previewbvox"
				v-show="previewSrcList.length && showpreview"
				@mousewheel="mousewheel"
			>
				<i class="el-icon-close icon" @click="closeView"></i>
				<img
					src="https://fuss10.elemecdn.com/8/27/f01c15bb73e1ef3793e64e6b7bbccjpeg.jpeg"
					alt=""
					class="previewimg"
					:style="scale"
				/>
			</div>
		</transition>
	</div>
</template>

<script>
	export default {
		props: {
			src: {
				type: String,
				required: true
			},
			previewSrcList: {
				type: Array
			}
		},
		data() {
			return {
				showpreview: false,
				params: {
					wheelVal: 1
				},
				scale: ""
			};
		},
		methods: {
			handleview() {
				this.showpreview = true;
				console.log(this.previewSrcList);
			},
			closeView() {
				this.showpreview = false;
			},
			//捕获滑轮滚动
			mousewheel(es) {
				let e = es || window.event;
				this.params.wheelVal += e.wheelDelta / 1200;
				if (this.params.wheelVal >= 0.2) {
					this.scale = `transform:scale(${this.params.wheelVal})`;
				} else {
					this.params.wheelVal = 0.2;
					this.scale = `transform:scale(${this.params.wheelVal})`;
					return false;
				}
			}
		}
	};
</script>

<style lang="scss" scoped>
	.box {
		width: 100%;
		height: 100%;
	}
	.imgbox {
		width: 100%;
		height: 100%;
		caret-color: rgba(0, 0, 0);
		img {
			display: block;
			caret-color: rgba(0, 0, 0);
			width: 100px;
			height: 100px;
			margin-left: 20%;
			margin-top: 20%;
		}
	}
	/* 蒙层 */
	.previewbvox {
		width: 100vw;
		height: 100vh;
		position: fixed;
		left: 0;
		top: 0;
		z-index: 9999;
		background-color: rgba(0, 0, 0, 20%);
		display: flex;
		justify-content: center;
		align-items: center;
		.icon {
			cursor: pointer;
			caret-color: rgba(0, 0, 0, 0);
			font-size: 30px;
			position: fixed;
			right: 40px;
			top: 80px;
			color: #fff;
			padding: 5px;
			border-radius: 50%;
			background-color: #606266;
		}
		.previewimg {
			max-width: 90vw;
			max-height: 90vh;
		}
	}
</style>

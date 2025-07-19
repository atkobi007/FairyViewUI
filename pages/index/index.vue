<template>
	<view>
		<image class="logo" src="/static/logo.png"></image>
		<text class="title">{{title}}</text>
		<fui-badge text="1"></fui-badge>
		<view style="width:300px;height:60px;position:relative; background: antiquewhite;">
			<fui-badge text="10" :isDot="false" :inverted="true" type="error" absolute="rightTop">
			</fui-badge>
		</view>
		<fui-card></fui-card>
		<fui-button></fui-button>
	</view>
</template>

<script setup>
	const title = ref("test")
	onMounted(() => {
		console.error("VUE")
	})
</script>

<style>
	.box {
		width: 40px;
		height: 60px;
		display: flex;
		justify-content: center;
		align-items: center;
		text-align: center;
		background-color: #DCDFE6;
		color: #fff;
		font-size: 12px;
	}

	.box-text {
		text-align: center;
		color: #fff;
		font-size: 12px;
	}

	.logo {
		height: 100px;
		width: 100px;
		margin: 100px auto 25px auto;
	}

	.title {
		font-size: 18px;
		color: #8f8f94;
		text-align: center;
	}
</style>
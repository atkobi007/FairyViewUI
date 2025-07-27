<template>
	<view class="fui-card" :style="radiusStyle">
		<slot>
			<view class="fui-card-template">
				<image class="img" src="../images/logo.png"></image>
				<view class="bottom">
					<text class="text">喜欢</text>
					<text class="text">倒踩</text>
					<text class="text">分享</text>
				</view>
			</view>
		</slot>
	</view>
</template>
<script setup lang="uts">
	/**
	 * @property shadowStyle:自定义阴影样式 示例:"0px 4px 8px 0px #AAA" 
	 *			#AAA 这里要写具体的色值,鸿蒙上 rgba 透明度不管用
	 * @property radius:圆角(默认6px) 示例：”12px 5px 2px 10px“【左上/右上/右下/左下】
	 * @property {string} width = [value] 
	 * @property {string} height = [value] 
	 */
	interface CardProps {
		shadowStyle : string
		radius : string
		width:string
		height:string
	}
	const props = withDefaults(defineProps<CardProps>(), {
		shadowStyle: "0rpx 8rpx 8rpx 4rpx #AAA",
		radius: "12rpx",
		width:"400rpx",
		height:"400rpx"
	})
	const radiusStyle = computed(() => {
		const style= {
			"border-radius": props.radius
		}
		if (props.shadowStyle.length > 0) {
			style['box-shadow'] = props.shadowStyle
		}
		if(props.width.length>0){
			style['width'] = props.width
		}
		if(props.height.length>0){
			style['height'] = props.height
		}
		return style
	})
</script>
<style lang="scss" scoped>
	@import "./index.scss"
</style>
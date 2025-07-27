<template>
	<view class="fui-button" :style="buttonStyle">
		<slot name="left">
			<image v-if="left" :style="leftIconStyle" class="fui-button-icon-left" :src="leftIcon"></image>
		</slot>
		<view class="fui-button-container">
			<slot name="top" v-if="top">
				<image :style="topIconStyle" class="fui-button-icon-left" :src="topIcon"></image>
			</slot>
			<slot class="fui-button-label" :class="buttonLabelClass">
				<text class="fui-button-label" :class="buttonLabelClass" :style="labelStyle">{{text}}</text>
			</slot>
			<slot name="bottom" v-if="bottom">
				<image :style="bottomIconStyle" class="fui-button-icon-left" :src="bottomIcon"></image>
			</slot>
		</view>
		<slot name="right" v-if="right">
			<image :style="rightIconStyle" class="fui-button-icon-right" :src="rightIcon"></image>
		</slot>
	</view>
</template>
<script setup lang="uts">
	import { ButtonSize, ButtonType, getButtonBackground } from "./fui-button.uts"

	/**
	 * @property {string} text = "按钮显示文字"
	 * @property {ButtonSize} size = "small"|"middle"|"large"|"auto"
	 * @property {ButtonType} type = "info" | "success" | "warning" | "error"|"primary"| "auto" 注意: type 设置后 background 设置无效
	 * @property {string} background = "按钮背景颜色" 默认 #AAA
	 * @property {string} labelColor = "文字颜色" 默认 #FFF
	 * @property {string} labelSize = "文字大小" 如：20px / 30rpx  默认 “” 
	 * @property {boolean} left = [false|true]左边icon 是否显示,  默认false
	 * @property {string} leftIcon = "左边ICON资源路径(要写全路径或 /static 目录下 ANDROID/HARMONY 下相对路径显示不出来图片)"
	 * @property {string} leftIconSize = "左边ICON大小,如 40px,60px, 默认40rpx"
	 * @property {boolean} right = [false|true]右边icon 是否显示,  默认false
	 * @property {string} rightIcon = "右边ICON资源路径(要写全路径或 /static 目录下 ANDROID/HARMONY 下相对路径显示不出来图片)"
	 * @property {string} rightIconSize = "右边ICON大小,如 40px,60px, 默认40rpx"
	 * @property {boolean} top = [false|true]上边icon 是否显示,  默认false
	 * @property {string} topIcon = "上边ICON资源路径(要写全路径或 /static 目录下 ANDROID/HARMONY 下相对路径显示不出来图片)"
	 * @property {string} topIconSize = "上边ICON大小,如 40px,60px, 默认40rpx"
	 * @property {boolean} bottom = [false|true]下边icon 是否显示,  默认false
	 * @property {string} bottomIcon = "下边ICON资源路径(要写全路径或 /static 目录下 ANDROID/HARMONY 下相对路径显示不出来图片)"
	 * @property {string} bottomIconSize = "下边ICON大小,如 40px,60px, 默认40rpx"
	 */

	interface ButtonProps {
		text : string
		size : ButtonSize
		type : ButtonType
		background : string
		labelColor : string
		labelSize : string
		left : boolean
		leftIcon : string
		leftIconSize : string
		right : boolean
		rightIcon : string
		rightIconSize : string
		top : boolean
		topIcon : string
		topIconSize : string
		bottom : boolean
		bottomIcon : string
		bottomIconSize : string
	}

	const props = withDefaults(defineProps<ButtonProps>(), {
		text: "BUTTON",
		size: "large",
		type: "auto",
		background: "#AAA",
		labelColor: "#FFF",
		labelSize: "",
		left: false,
		leftIcon: "/static/logo.png",
		leftIconSize: "40rpx",
		right: false,
		rightIcon: "/static/logo.png",
		rightIconSize: "40rpx",
		top: false,
		topIcon: "/static/logo.png",
		topIconSize: "40rpx",
		bottom: false,
		bottomIcon: "/static/logo.png",
		bottomIconSize: "40rpx"
	})
	//----------------------  BUTTON 样式设置 Start --------------------------------
	const buttonStyle = computed<UTSJSONObject>(() => {
		const type : ButtonType = props.type;
		const buttonObj : UTSJSONObject = new UTSJSONObject();
		buttonObj.set("background", getButtonBackground(type, props.background))
		return buttonObj;
	})
	//----------------------  BUTTON 样式设置 End --------------------------------

	//----------------------  LABEL 样式设置 Start --------------------------------
	const buttonLabelClass = computed<Array<string>>(() => {
		const size = props.size
		return [`fui-button-label-${size}`]
	})
	const labelStyle = computed<UTSJSONObject>(() => {
		const labelColor = props.labelColor;
		const labelSize : string = props.labelSize;
		const labelStyle : UTSJSONObject = new UTSJSONObject();
		labelStyle.set("color", labelColor);
		if (labelSize.length > 0) {
			labelStyle.set("font-size", labelSize)
		}
		return labelStyle
	})
	//----------------------  LABEL 样式设置 End --------------------------------

	//----------------------  ICON 大小设置 Start --------------------------------
	const getSizeObj = (size : string) : UTSJSONObject => {
		const sizeObj : UTSJSONObject = new UTSJSONObject();
		sizeObj.set("width", size);
		sizeObj.set("height", size);
		return sizeObj;
	}
	const leftIconStyle = computed(() => {
		const size = props.leftIconSize
		return getSizeObj(size);
	})
	const rightIconStyle = computed(() => {
		const size = props.rightIconSize
		return getSizeObj(size);
	})
	const topIconStyle = computed<UTSJSONObject>(() => {
		const size = props.topIconSize;
		return getSizeObj(size);
	})
	const bottomIconStyle = computed<UTSJSONObject>(() => {
		const size = props.leftIconSize
		return getSizeObj(size);
	})
	//----------------------  ICON 大小设置 End --------------------------------
</script>
<style lang="scss" scoped>
	@import "@/uni_modules/fui-style/index.scss";

	.fui-button {
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		border-radius: $fui-size-10;
		padding: $fui-size-10 $fui-size-10;
	}

	.fui-button-container {
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	.fui-button-label {
		text-align: center;
	}

	.fui-button-label-small {
		font-size: $fui-size-20;
	}

	.fui-button-label-middle {
		font-size: $fui-size-30;
	}

	.fui-button-label-large {
		font-size: $fui-size-40;
	}

	.fui-button-icon-left {
		margin-right: $fui-size-5;
	}

	.fui-button-icon-right {
		margin-left: $fui-size-5;
	}

	.fui-button-icon-top {
		margin-left: $fui-size-5;
	}

	.fui-button-icon-bottom {
		margin-left: $fui-size-5;
	}
</style>
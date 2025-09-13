<template>
	<view class="safe" :style="style"></view>
</template>
<script lang="uts" setup>
	/**
	 * @property {string} width = [value] 
	 * @property {string} height = [value] 
	 * @property {string} backGroundColor = [value]
	 * @property {UTSJSONObject} customClass = [value]  自定义样式
	 * @property {boolean} safeAreaTop = [value]		顶部安全区域
	 * @property {boolean} safeAreaRight = [value]		右边安全区域
	 * @property {boolean} safeAreaBottom = [value]		底部安全区域
	 * @property {boolean} safeAreaLeft = [value]		左边安全区域
	 */
	interface FuiGapProps {
		width : string
		height : string
		backGroundColor : string
		customClass : UTSJSONObject
		safeAreaTop : boolean
		safeAreaRight : boolean
		safeAreaBottom : boolean
		safeAreaLeft : boolean
	}
	const props = withDefaults(defineProps<FuiGapProps>(), {
		width: "100%",
		height: "5rpx",
		backGroundColor: "",
		customClass: new UTSJSONObject({ "background": "red" }),
		safeAreaTop: true,
		safeAreaRight: true,
		safeAreaBottom: true,
		safeAreaLeft: true,
	})

	const style = computed(() => {
		let styleObject = new UTSJSONObject(props.customClass);
		styleObject.set("width", props.width);
		styleObject.set("height", props.height);
		styleObject.set("background", props.backGroundColor);
		//优先customClass自定义设置
		for (let key in props.customClass) {
			styleObject.set(key, props.customClass.get(key))
		}
		if (props.safeAreaBottom || props.safeAreaTop || props.safeAreaRight || props.safeAreaLeft) {
			const systemInfo = uni.getSystemInfoSync();
			const safeInfo = systemInfo.safeAreaInsets;
			styleObject.set("padding-top", `${safeInfo.top}px`);
			styleObject.set("padding-right", `${safeInfo.right}px`);
			styleObject.set("padding-bottom", `${safeInfo.bottom}px`);
			styleObject.set("padding-left", `${safeInfo.left}px`);
		}
		return styleObject
	})

	onMounted(() => {
		
	})
</script>
<style lang="scss" scoped>
</style>
<script setup>
	/**
	 * @property {string} color = [value] 
	 * @property {string} name = [value] 
	 * @property {string} customFontFamily = [value]  字定义字体文件路径
	 * @property {number} size = [value]		大小
	 */
	interface FuiIconProps {
		color: string
		name: string
		customFontFamily: string
		size: number
	}
	const props = withDefaults(defineProps < FuiIconProps > (), {
		color: "red",
		name: "checkbox-unchecked",
		customFontFamily: "",
		size: 40,
	})
	import {
		FuiIcon
	} from "./fui-icon.uts";
	const emits = defineEmits(['click'])

	/**
	 * 获取图标
	 */
	const getFontName = computed((): any => {
		const name = `icon-${props.name}`
		const icon = FuiIcon[name];
		if (icon == null) {
			return props.name
		}
		return icon
	})
	/**
	 * 获取图标资源
	 */
	const getFontSource = computed((): string => {
		return props.customFontFamily == "" ? "/uni_modules/fairy-ui/static/fui-icon.ttf" : props.customFontFamily
	})
	/**
	 * 获取图标名称
	 */
	const getFontFamily = computed((): string => {
		return "fui-icon"
	})
	/**
	 * 获取图标颜色
	 */
	const getFontColor = computed((): string => {
		let color = props.color
		return color
	})
	/**
	 * 获取图标大小
	 */
	const getFontSize = computed((): string => {
		return props.size + "rpx"
	})
	/**
	 * 获取样式
	 */
	const getStyle = computed((): UTSJSONObject => {
		let style: UTSJSONObject = new UTSJSONObject();
		style.set("font-family", getFontFamily.value)
		style.set("color", getFontColor.value)
		style.set("font-size", getFontSize.value)
		return style
	})


	const loadFont = () => {
		const fontFamily = getFontFamily.value;
		const fontSource = getFontSource.value
		uni.loadFontFace({
			global: false,
			family: fontFamily,
			source: `url('${fontSource}')`,
			success: () => {
				console.log(`load ${fontFamily} success`)
			},
			fail: (err) => {
				console.error(`load ${fontFamily} ${err}`)
			}
		})
	}

	const onClick = (event: MouseEvent) => {
		emits('click', event)
	}

	onMounted(() => {
		loadFont()
	})
</script>

<template>
	<text :style="[getStyle]" @click="onClick">
		{{ getFontName }}
	</text>
</template>

<style lang="scss" scoped>
	.fui-icon{
		display: flex;
		flex-direction: row;
	}
</style>
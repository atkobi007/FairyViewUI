<template>
	<view class="fui-text">
		<slot name="prefix">
			<!-- <view>前置</view> -->
		</slot>
		<slot>
			<text :style="getStyle()" style="flex:1;">
				<text @click="onClickHightLight(item)" v-for="item in showList" :key="item.id"
					:style="getSubTextStyle(item)">{{item.text}}</text>
			</text>
		</slot>
		<slot name="suffix">
			<!-- <view>后缀</view> -->
		</slot>
	</view>
</template>
<script setup lang="uts">
	import { FuiTextUtils, FuiTextResult, IFuiTextProps } from "./fui-text.uts"

	const emit = defineEmits(["click-high-light"])

	/**
	 * 说明:如果外部重写默认slot 则,所有高亮相关的失效,只能当普通文本使用
	 * @property {String} text = [value] 显示的文字【备注:如果全是英文或数据的时间有可能 app 上换行有bug,会整体单词换行】
	 * @property {String} textColor = [value] 显示的正常文字颜色
	 * @property {String} highLightText = [value]  需要高亮的文字,如果有多个需要高亮的以逗号分隔
	 * @property {String} hightLightColor = [value] 高亮文字颜色
	 * @property {Number} fontSize = [value]  字体大小
	 * @property {Number} lines = [value]  行数 默认0 自动高度,超出无 ... 显示,设置 ellipsis 后当前属性无效
	 * @property {String} ellipsis = [value] 设置后默认只显示一行(lines无效),超出后显示 ...  目前只支持单行【多行待TODO】
	 * @slot  prefix 前置插槽【自定义】
	 * @slot  suffix 前置插槽【自定义】
	 */
	interface FuiTextProps {
		text : string
		textColor : string
		backGroundColor : string
		highLightText : string
		hightLightColor : string
		hightLightBackGroundColor : string
		fontSize : number
		lines : number
		ellipsis : string
	}
	const props = withDefaults(defineProps<FuiTextProps>(), {
		text: "这是是所有的文字内容,可以显示高亮文字",
		textColor: "blue",
		backGroundColor: "",
		highLightText: "高亮",
		hightLightColor: "red",
		hightLightBackGroundColor: "",
		fontSize: 26,
		lines: 0,
		ellipsis: ""
	})

	const showList = ref<FuiTextResult[]>([])

	const getStyle = () => {
		let obj:UTSJSONObject= new UTSJSONObject()
		obj.set("fontSize",props.fontSize);
		obj.set("lines",props.lines);
		obj.set("ellipsis",props.ellipsis);
		return FuiTextUtils.getStyle(obj);
	}
	const getSubTextStyle = (item : FuiTextResult) : UTSJSONObject => {
		let obj:UTSJSONObject= new UTSJSONObject()
		obj.set("fontSize",props.fontSize);
		obj.set("textColor",props.textColor);
		obj.set("backGroundColor",props.backGroundColor);
		obj.set("hightLightColor",props.hightLightColor);
		obj.set("hightLightBackGroundColor",props.hightLightBackGroundColor);
		obj.set("ellipsis",props.ellipsis);
		let subStyle = FuiTextUtils.getSubTextStyle(item, obj)
		return subStyle;
	}

	const onClickHightLight = (item : FuiTextResult) => {
		uni.showToast({
			title: item.text
		})
		emit("click-high-light", item.text)
	}

	onMounted(() => {
		//先拆分出来需要高亮的文字
		const hlText = props.highLightText
		showList.value = FuiTextUtils.splitByKeywords(props.text, hlText)
	})
</script>
<style lang="scss">
	.fui-text {
		display: flex;
		flex-direction: row;
		align-items: center;
	}
</style>
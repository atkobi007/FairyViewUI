<template>
	<view class="fui-badge-root">
		<slot>
			<template>
				<view class="template">1</view>
			</template>
		</slot>
		<text v-if="text" :class="classNames" :style="[positionStyle, customStyle, dotStyle]"
			class="fui-badge fui-badge--absolute">{{displayValue}}</text>
	</view>
</template>

<script setup lang="uts">
	import fuiState from "@/uni_modules/fairy-ui/fui-core/index.uts"
	import { getPos, getDotResult, getDisplayValue } from './badge';

	/**
	 * Badge 数字角标  包裹的父节点必须设置 position
	 * @description 数字角标一般和其它控件（列表、9宫格等）配合使用，用于进行数量提示，默认为实心灰色背景
	 * @property {String} text 角标内容
	 * @property {String} type = [info|primary|success|warning|error] 颜色类型
	 * 	@value info 灰色
	 * 	@value primary 蓝色
	 * 	@value success 绿色
	 * 	@value warning 黄色
	 * 	@value error 红色
	 * @property {String} inverted = [true|false] 是否需背景颜色
	 * @property {Number} maxNum 展示封顶的数字值，超过 99 显示 99+
	 * @property {String} absolute = [rightTop|rightBottom|leftBottom|leftTop] 开启绝对定位, 角标将定位到其包裹的标签的四角上
	 * 	@value rightTop 右上
	 * 	@value rightBottom 右下
	 * 	@value leftTop 左上
	 * 	@value leftBottom 左下
	 * @property {Array[number]} offset	距定位角中心点的偏移量，只有存在 absolute 属性时有效，例如：[-10, -10] 表示向外偏移 10px，[10, 10] 表示向 absolute 指定的内偏移 10px
	 * @property {String} isDot = [true|false] 是否显示为一个小点
	 * @event {Function} click 点击 Badge 触发事件
	 * @example <fui-badge text="1"></fui-badge>
	 */
	interface FuiBadgeProps {
		type : string
		inverted : boolean
		isDot : boolean
		maxNum : number
		absolute : string
		offset : Array<number>
		text : string
		customStyle : UTSJSONObject
	}
	const props = withDefaults(defineProps<FuiBadgeProps>(), {
		type: 'error',
		inverted: true,
		isDot: false,
		maxNum: 99,
		absolute: "rightTop",
		offset: [0, 0],
		text: "N",
		customStyle: new UTSJSONObject()
	})

	const classNames = computed(() => {
		const typeStr : string = props.type;
		const type : string = 'fui-badge--' + typeStr;
		const invertedBoo : boolean = props.inverted;
		const absoluteBoo : boolean = props.absolute.length > 0
		const inverted = invertedBoo ? type + '-inverted' : type;
		const absolute = absoluteBoo ? 'fui-badge--absolute' : '';
		const cls = [
			inverted, absolute
		].join(' ')
		return cls;
	});

	const positionStyle = computed(() => {
		return getPos(props.absolute, props.offset);
	})

	const dotStyle = computed(() => {
		return getDotResult(props.isDot)
	});

	const displayValue = computed(() => {
		if (props.isDot) return ""
		const text : string = props.text;
		return getDisplayValue(text, 99)
	})
</script>

<style lang="scss" scoped>
	@import "@/uni_modules/fairy-ui/fui-style/index.scss";

	.fui-badge-root {
		position: relative;
		margin: 0px auto;
		padding: 10rpx 10rpx;

		.template {
			width: 50rpx;
			height: 50rpx;
			background: #EEE;
		}

		.fui-badge {
			padding: 0 $fui-size-4;
			border-radius: $fui-size-40;
			text-align: center;
			font-size: $fui-size-20;
			z-index: 10;

			&--absolute {
				position: absolute;
			}

			&--info {
				background: transparent;
				border: 1px solid $fui-info;
				color: $fui-info;
			}

			&--primary {
				background: transparent;
				border: 1px solid $fui-primary;
				color: $fui-primary;
			}

			&--success {
				background: transparent;
				border: 1px solid $fui-success;
				color: $fui-success;
			}

			&--warning {
				background: transparent;
				border: 1px solid $fui-warning;
				color: $fui-warning;
			}

			&--error {
				background: transparent;
				border: 1px solid $fui-error;
				color: $fui-error;
			}

			&--info-inverted {
				background: $fui-info;
				border: 1px solid $fui-info;
				color: $fui-default;
			}

			&--primary-inverted {
				background: $fui-primary;
				border: 1px solid $fui-primary;
				color: $fui-default;
			}

			&--success-inverted {
				background: $fui-success;
				border: 1px solid $fui-success;
				color: $fui-default;
			}

			&--warning-inverted {
				background: $fui-warning;
				border: 1px solid $fui-warning;
				color: $fui-default;
			}

			&--error-inverted {
				background: $fui-error;
				border: 1px solid $fui-error;
				color: $fui-default;
			}
		}
	}
</style>
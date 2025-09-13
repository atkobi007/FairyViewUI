<template>
	<view class="fui-image" :style="style">
		<image :src="imageSrc" v-if="showImage" :mode="mode" @error="onError" @load="onLoad"
			style="width: 100%;height: 100%;">
		</image>
		<view v-if="isLoading" class="fui-image-loading">
			<slot name="loading">
				<text class="text">加载中</text>
			</slot>
		</view>
	</view>
</template>
<script setup lang="uts">
	/**
	 * 备注: src|errorSrc 本地目录只支持根目录下的 /static/ 下的资源路径
	 * @property {string} src = [value]			资源路径
	 * @property {string} errorSrc = [value] 	加载出错时显示的资源路径
	 * @property {string} width = [value] 		宽度
	 * @property {string} height = [value]		高度
	 * @property {string|number} radius = ["50rpx 10rpx 20rpx 30rpx" | 60]
	 * @property {ModeType} mode = [value] 
	 * @property {UTSJSONObject} customClass = [value]  自定义样式,优先显示
	 * @property {boolean} loading = [value] 是否显示加载中
	 * @event {Function(e:UniImageErrorEvent)} error  	(e:UniImageErrorEvent)=>{}
	 * @event {Function(e:UniImageLoadEvent)} load 		(e:UniImageLoadEvent)=>{}
	 */

	/**
	 * 图片显示模式 默认值 scaleToFill 具体看官方支持,以下暂不支持hm
	 * @property scaleToFill: 不保持纵横比缩放图片，使图片的宽高完全拉伸至填满 image 元素
	 * @property aspectFit: 保持纵横比缩放图片，使图片的长边能完全显示出来。也就是说，可以完整地将图片显示出来。
	 * @property aspectFill: 保持纵横比缩放图片，只保证图片的短边能完全显示出来。也就是说，图片通常只在水平或垂直方向是完整的，另一个方向将会发生截取
	 * @property widthFix: 宽度不变，高度自动变化，保持原图宽高比不变
	 * @property heightFix: 高度不变，宽度自动变化，保持原图宽高比不变
	 * @property top: 不缩放图片，只显示图片的顶部区域
	 * @property bottom: 不缩放图片，只显示图片的底部区域
	 * @property center: 不缩放图片，只显示图片的中间区域
	 * @property left: 不缩放图片，只显示图片的左边区域
	 * @property right: 不缩放图片，只显示图片的右边区域
	 * @property top left: 不缩放图片，只显示图片的左上边区域
	 * @property top right: 不缩放图片，只显示图片的右上边区域
	 * @property bottom left: 不缩放图片，只显示图片的左下边区域
	 * @property bottom right: 不缩放图片，只显示图片的右下边区域
	 *
	 */

	const emit = defineEmits(["error", "load"])

	type ModeType = "scaleToFill" | "aspectFit" | "aspectFill" | "widthFix" | "heightFix" | "top" | "bottom" | "center" | "left" | "right" | "top left" | "top right" | "bottom left" | "bottom right"

	interface FuiImageProps {
		src : string
		errorSrc : string
		width : string
		height : string
		radius : string | number
		mode : ModeType,
		loading : boolean
		customClass : UTSJSONObject
	}
	const props = withDefaults(defineProps<FuiImageProps>(), {
		src: "https://img.iplaysoft.com/wp-content/uploads/2019/free-images/free_stock_photo1.jpg",
		errorSrc: "/uni_modules/fairy-ui/static/error.jpg",
		width: "100rpx",
		height: "100rpx",
		radius: 16,
		mode: "scaleToFill",
		loading: true,
		customClass: new UTSJSONObject(),//{ "border-radius": "24px" }
	})

	const imageSrc = ref("");
	//是识是否加载失败
	const isError = ref(false);
	const isLoading = ref(false);
	const showImage = computed(() => {
		return imageSrc.value.length > 0
	})

	onBeforeMount(() => {
		isLoading.value = props.loading
		if (props.src.length <= 0) {
			throw new Error("必须传入 src 资源路径")
		}
		isError.value = false
		imageSrc.value = props.src
	})

	const style = computed(() => {
		let styleObject = new UTSJSONObject(props.customClass);
		styleObject.set("width", props.width);
		styleObject.set("height", props.height);

		const radius = props.radius;
		if (typeof radius === "number") {
			styleObject.set("border-radius", `${radius}rpx`);
		} else {
			styleObject.set("border-radius", radius);
		}

		//优先customClass自定义设置
		const customClass : UTSJSONObject = props.customClass
		for (let key in customClass) {
			styleObject.set(key, customClass.get(key))
		}
		return styleObject
	})

	//加载失败
	const onError = (e : UniImageErrorEvent) => {
		//是当前资源图片加载失败了,显示为失败的资源
		imageSrc.value = props.errorSrc;
		isError.value = true;
		emit("error", e);
	}
	//app-android平台由于默认启用了图片缩放（即根据组件实际宽高加载图片，以节省内存），所以可能导致load事件返回的图片尺寸并非图片原始尺寸
	//图片加载完成时触发，event.detail = { width: '图片宽度px', height: '图片高度px' }
	const onLoad = (e : UniImageLoadEvent) => {
		isLoading.value = false;
		emit("load", e);
	}
</script>
<style scoped lang="scss">
	@import "@/uni_modules/fairy-ui/fui-style/index.scss";
	.fui-image {
		background:#EEE;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		position: relative;
		&-loading{
			position: absolute;
			top:0;
			width:100%;
			height:100%;
			display: flex;
			flex-direction: column;
			align-items: center;
			justify-content: center;
			.text{
				font-size: $fui-size-24;
				color:#CCC
			}
		}
	}
</style>
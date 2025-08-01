<template>
	<view class="fui-swiper-item" :style="bgStyle">
		{{title}}-{{index}}--{{active}}
	</view>
</template>

<script lang="uts" setup>
	interface SwiperItemProps {
		title : string
		src : string
		index : number
		active : boolean
	}

	const props = withDefaults(defineProps<SwiperItemProps>(), {
		title: "",
		src: "",
		index: 0,
		active: true,
	})

	const bgStyle = computed(() => {
		let obj : UTSJSONObject = new UTSJSONObject();
		obj.set("background", `${props.active ? 'red' : 'gray'}`);
		return obj;
	})
</script>

<style lang="scss" scoped>
	.fui-swiper-item {
		width: 100%;
		height: 100%;
	}
</style>
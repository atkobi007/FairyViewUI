<script setup lang="uts">
	import swiper_item from "./fui-swiper-item.vue"
	
const emits = defineEmits(["change","transition","animationfinish"])

type SwiperItem={
	id:string
	name:string
	src:string
}

interface SwiperProps{
	d3:boolean
	list:Array<SwiperItem>
	dot:boolean
	color:string
	activeColor:string
	touch:boolean
	autoplay:boolean
	current:number
	interval:number
	circular:boolean
	vertical:boolean
}
const props=withDefaults(defineProps<SwiperProps>(),{
	d3:false,
	list:new Array<SwiperItem>(),
	dot:true,
	color:"#FFF",
	activeColor:"yellow",
	touch:false,
	autoplay:true,
	current:0,
	interval:30000,
	circular:true,
	vertical:false
})
const activeIndex = ref<number>(0)
const currentItemId = ref("")
const swiperList=ref<Array<SwiperItem>>([])
const isApp=ref(true)

const setCurrent = computed((): Number => {
	return activeIndex.value
})

const change = (event: UniSwiperChangeEvent) => {
	activeIndex.value = event.detail.current;
	emits("change", event)
}
const transition = (event: UniSwiperTransitionEvent) => {
	emits("transition", event)
}
const animationfinish = (event: UniSwiperAnimationFinishEvent) => {
	emits("animationfinish", event)
}

const d3Margin=computed(()=>{
	let marginStr="0rpx"
	// #ifdef WEB
		marginStr= props.d3?"60rpx":"0rpx";
	// #endif
	return marginStr;
})



onMounted(()=>{
	// #ifdef WEB
		isApp.value =false;
	// #endif
	
	let dataList:Array<SwiperItem>=[];
	for(let i=0;i<4;i++){
		let info:SwiperItem={id:i+"",name:"item"+i,src:""};
		dataList.push(info);
	}
	swiperList.value=dataList
})

</script>

<template>
	<swiper
		style="width:100%;height:100%;"
		ref="xSwiper"
		:indicator-dots="dot"
		:indicator-color="color"
		:indicator-active-color="activeColor"
		:interval="interval"
		:disable-touch="touch"
		:autoplay="autoplay"
		:current="setCurrent"
		:current-item-id="currentItemId"
		:circular="true"
		:previous-margin="d3Margin"
		:next-margin="d3Margin"
		:vertical="vertical"
		@change="change"
		@transition="transition"
		@animationfinish="animationfinish"
	>
		<swiper-item style="width:100%;height:100%;" v-for="(data, index) in swiperList" :key="index">
			<view style="width:100%;height:100%;" :style="{'transform':`${isApp||!d3?'scale(1)': (activeIndex==index)?'scale(1)':'scale(0.9)'}`}">
				<slot :title="data.name" :index="index" :active="activeIndex==index">
					<swiper_item :title="data.name" :active="activeIndex==index" :index="index" />
				</slot>
			</view>
		</swiper-item>
	</swiper>
</template>

<style>

</style>
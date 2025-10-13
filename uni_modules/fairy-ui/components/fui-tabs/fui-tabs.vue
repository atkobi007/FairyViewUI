<template>
	<view class="fui-tabs">
		<view class="fui-tabs-container" id="tabContainer">
			<view class="fui-tabs-item" @click="tabChanged(i)" v-for="(item,i) in [1,1222,13333,4441,51,16785333,1345,133,1,1333,1,134534535,1,4444441,3453453451]">
				<view>ITEM-{{item}}</view>
				<view :class="{'active':currentIndex==i}"></view>
			</view>
		</view>
	</view>
</template>

<script setup>
	const currentIndex = ref(0);
	const query = uni.createSelectorQuery().in(this);
	let tabContainer: UniElement | null;
	let itemInfos: NodeInfo[] = []

	onMounted(() => {
		nextTick(() => {
			query
				.selectAll(`.fui-tabs-item`)
				.boundingClientRect((data: NodeInfo[]) => {
					console.log("得到布局位置信息", data);
					itemInfos = data;
				})
				.exec();
			tabContainer = uni.getElementById("tabContainer");
		})
	})

	const tabChanged = (index: number) => {
		if (index === currentIndex.value) return;
		currentIndex.value = index;
		console.error(index);

		updatePosition();
	}

	const updatePosition = () => {
		if (tabContainer == null) return;
		let total = 0;
		for (let i = 0; i < itemInfos.length; i++) {
			if (itemInfos[i] == null) continue;
			total += itemInfos[i].width
			if (i == currentIndex.value) {
				break
			}
		}
		let scrollLeft = 0;
		let centerX = tabContainer.offsetWidth / 2;
		if (total > centerX) {
			scrollLeft = total - centerX;
		}
		tabContainer.scrollTo(scrollLeft, 0)
	}
</script>

<style scoped>
	.fui-tabs {
		width: 100%;
		height: 100rpx;
		background: yellowgreen;
		overflow: hidden;
	}

	.fui-tabs-container {
		height: 100rpx;
		display: flex;
		flex-direction: row;
		align-items: center;
		overflow: auto;
		position: relative;
	}

	.fui-tabs-container::-webkit-scrollbar {
		display: none;
	}

	.fui-tabs-item {
		padding: 0px 10px;
		display: flex;
		flex-direction: column;
		position: relative;
		height: 100rpx;
		align-items: center;
		justify-content: center;
	}

	@keyframes widthAnimation {
		0% {
			width: 10rpx;
		}

		/* 动画开始时 */
		50% {
			width: 30rpx;
		}

		/* 动画中间状态 */
		100% {
			width: 60rpx;
		}

		/* 动画结束时 */
	}

	.active {
		background: aqua;
		width: 60rpx;
		height: 4rpx;
		background: blue;
		position: absolute;
		bottom: 0;


		/* #ifdef WEB */
		animation-name: widthAnimation;
		/* 应用名为 widthAnimation 的动画 */
		animation-duration: 0.1s;
		/* 动画持续时间 */
		animation-timing-function: linear;
		/* 动画速度曲线 */
		animation-iteration-count: 1;
		/* 动画播放次数，这里设置为无限循环 */
		/* #endif */


	}
</style>
<template>
	<div class="fui-card">
		<slot>
			<div class="fui-card-template">
				<image class="img" src="../images/logo.png"></image>
				<div class="bottom">
					<div>喜欢</div>
					<div>倒踩</div>
					<div>分享</div>
				</div>
			</div>
		</slot>
	</div>
</template>
<script>
</script>
<style lang="scss" scoped>
	.fui-card {
		width: 300px;
		height: 300px;
		padding: 5px;
		background-color: #FEFEFE;
		border-radius: 6px;
		// #AAA 这里要写具体的色值,鸿蒙上 rgba 透明度不管用
		box-shadow: 0px 4px 8px 0px #AAA;

		//示例
		&-template {
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;
			height: 100%;
			width: 100%;
			.img {
				width: 100%;
				height: 260px;
			}
			.bottom {
				flex: 1;
				width: 100%;
				display: flex;
				flex-direction: row;
				align-items: center;
				justify-content: space-between;
			}
		}
	}
</style>
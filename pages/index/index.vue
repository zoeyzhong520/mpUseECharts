<template>
	<view class="content">

		<!-- 雷达图 -->
		<view class="charts-box">
			<mpvue-echarts @onInit="lineInit" canvasId="radar" ref="lineChart" />
		</view>

	</view>
</template>

<script>
	import * as echarts from '../../components/echarts/echarts.min.js'
	import mpvueEcharts from '../../components/mpvue-echarts/src/echarts.vue'

	export default {
		components: {
			mpvueEcharts
		},

		data() {
			return {
				// ECharts 雷达图数据
				chartData: {
					"backgroundColor": "#FFFFFF",
					"radar": {
						"axisLine": {
							"lineStyle": {
								"color": "#E4EEFF",
								"width": 1
							}
						},
						"indicator": [{
								"max": 10,
								"name": "研发能力"
							},
							{
								"max": 10,
								"name": "综合能力"
							},
							{
								"max": 10,
								"name": "配套实力"
							},
							{
								"max": 10,
								"name": "成本控制能力"
							},
							{
								"max": 10,
								"name": "生产能力"
							}
						],
						"name": {
							"color": "#333333",
							"fontSize": 13
						},
						"splitArea": {
							"areaStyle": {
								"color": [
									"rgba(63,118,246,0.08)"
								]
							}
						},
						"splitLine": {
							"lineStyle": {
								"color": [
									"#EAF1FC"
								],
								"width": 1
							}
						},
						"splitNumber": 0
					},
					"series": [{
						"areaStyle": {
							"color": "#3F76F6",
							"opacity": 1,
							"shadowBlur": 5,
							"shadowColor": "rgba(0, 33, 109, 0.16)",
							"shadowOffsetY": 2
						},
						"data": [{
							"symbol": "none",
							"value": [
								9.38,
								9.75,
								9,
								7.88,
								8.88
							]
						}],
						"lineStyle": {
							"width": 0
						},
						"type": "radar"
					}],
					"xAxis": {
						"show": false
					},
					"yAxis": {
						"show": false
					}
				},
			}
		},
		onLoad() {

		},
		methods: {
			// 绘制 ECharts
			lineInit(e) {
				let {
					width,
					height
				} = e;
				let canvas = this.$refs.lineChart.canvas
				echarts.setCanvasCreator(() => canvas);
				this.lineChart = echarts.init(canvas, null, {
					width: width,
					height: height
				})
				canvas.setChart(this.lineChart)
				this.lineChart.setOption(this.chartData)
				this.$refs.lineChart.setChart(this.lineChart)
			},
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	/* 雷达图父容器 */
	.charts-box {
		width: 670rpx;
		height: 439rpx;
		padding: 20rpx 0 0 0;
	}
</style>

# 小程序如何使用 ECharts 绘制雷达图

## 前言
众所周知，在H5平台绘制图表首选 ECharts 库。它具有以下特色：丰富的可视化类型、多种数据无需转换直接使用、移动端优化、动态数据、绚丽的特效、GL实现三维可视化。这里放上 5分钟上手ECharts 官方文档入口，感兴趣的朋友们可以一睹为先。

不过本篇文章Joe大叔将要带领大家一步步的在微信小程序中使用 ECharts 成功绘制出雷达图，接下来我们就开始吧！

## 一、微信小程序如何接入 ECharts？​​​​​​​
这里要区分两种情况：一是原生微信小程序开发，这里Joe大叔推荐大家直接访问 [在微信小程序中使用ECharts](https://echarts.apache.org/zh/tutorial.html#%E5%9C%A8%E5%BE%AE%E4%BF%A1%E5%B0%8F%E7%A8%8B%E5%BA%8F%E4%B8%AD%E4%BD%BF%E7%94%A8%20ECharts)，因为是官方文档，只需要按照操作步骤即可。二是 uni-app 开发微信小程序，下面我将展示使用 uni-app项目 绘制雷达图。

## 二、使用步骤
### 1.引入库
#### 1.1 使用 HBuilder X 新建 uni-app 项目，并选择 Hello uni-app 模版。



 项目创建完毕，在 components 目录下找到 mpvue-echarts 目录。



接着创建一个新项目，命名为 uni-app使用ECharts绘制雷达图，并把 mpvue-echarts 目录拷贝到创建的新项目中，目录结构如图所示即可。

![image](https://user-images.githubusercontent.com/22392094/131482511-8d0ae07d-8119-4f14-9a7e-6a8132426d06.png)


#### 1.2 前往 Echarts 官网[在线构建所需图标、坐标系、组件进行打包下载](https://echarts.apache.org/zh/builder.html)，由于本文是绘制雷达图，所以只需勾选雷达图即可。



点击下载按钮，打包完成后自动完成下载 js 文件。
![image](https://user-images.githubusercontent.com/22392094/131482624-05a300d4-82d6-4bd8-b102-eca7485c5d85.png)



新建 echarts 目录，把下载好的 echarts.min.js 文件拖入项目中。

 ![image](https://user-images.githubusercontent.com/22392094/131482683-a505752c-1e2a-4ae3-9118-1325829d170a.png)


#### 1.3 在 index.vue 里引入 js 文件和 mpvueEcharts 组件，并且进行组件注册。
```
<script>
	import * as echarts from '../../components/echarts/echarts.min.js'
	import mpvueEcharts from '../../components/mpvue-echarts/src/echarts.vue'
	
	export default {
		components: {
			mpvueEcharts
		},
		
		data() {
			return {
				title: 'Hello'
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>
```
至此，我们已经完成了 uni-app 项目中引入 ECharts 插件的步骤，接下来开始使用插件绘制雷达图。

### 2.ECharts绘制雷达图
#### 2.1 给雷达图创建一个父容器 charts-box，并设置宽高
```
<template>
	<view class="content">
		
		<!-- 雷达图 -->
		<view class="charts-box">
			<mpvue-echarts @onInit="lineInit" canvasId="radar" ref="lineChart" />
		</view>
		
	</view>
</template>

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
	  height:439rpx;
	  padding: 20rpx 0 0 0;
	}
</style>
```
这里的 @onInit 是 mpvue-echarts/src/echarts.vue 里的 echarts.vue 初始化 canvas 后的回调，我们在这里进行 ECharts 的数据提供和图形绘制。
```
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
```
#### 2.2 配置 ECharts 数据，这里Joe大叔已经准备了一个雷达图的配置数据，直接展现给大家。
```
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
```
在这里简要说明一下几个主要配置字段，更多详细内容可以直接访问 [ECharts 配置项](https://echarts.apache.org/zh/option.html#title)进行查阅。

**radar.axisLine**：坐标轴轴线相关设置。

**radar.name**：雷达图每个指示器名称的配置项。

**radar.splitArea**：坐标轴在 grid 区域中的分割区域，默认不显示。

**radar.splitLine**：坐标轴在 grid 区域中的分隔线。

**radar.splitNumber**：指示器轴的分割段数。（如果设为0，就只会展示一层雷达图背景）

**seris-radar.data.symbol**：雷达图的单个数据标记的图形。（如果设为none，就不会展示单个数据项的标记图形，默认是circle小圆点）

**seris-radar.data.value*：雷达图的单个数据的数值。（雷达图最终渲染效果就是由这个值来确定的，radar.indicator 里的 max 是 value 的最大取值，这里需要注意一下）

#### 2.3 编译项目，进行雷达图绘制。
![image](https://user-images.githubusercontent.com/22392094/131482763-9c8547b9-90c3-4296-9524-c65def9e8d72.png)


看完本文、跟随步骤后，一个漂亮的雷达图就呈现在眼前了，大家是不是也都跃跃欲试了呢？[ECharts 示例](https://echarts.apache.org/examples/zh/index.html)提供了各式各样漂亮的图表，足以满足各项要求，可以依据兴趣分别尝试下各个图表。



## 总结
在本篇文章中，Joe大叔和大家一起构建项目、引入 ECharts 插件、使用插件绘制图表，最终完成了达到预期效果的雷达图。当然，微信小程序在使用 ECharts 时，也存在 canvas 层级过高的问题。处理这个问题有两种方案：一是使用[cover-view、cover-image](https://uniapp.dcloud.io/component/cover-view?id=cover-view) 覆盖在图标上面；二是使用 canvas 生成图片[（uni.canvasToTempFilePath(object, component）](https://uniapp.dcloud.io/api/canvas/canvasToTempFilePath)，然后把图片展示出来，并且对 canvas 组件使用 position: absolute;left: -670rpx;right: -440rpx; 的布局方式将其隐藏在屏幕外。

最后很感谢能看到文末的朋友们，我们下期再见！

​
[CSDN博客地址](https://blog.csdn.net/baidu_35383008/article/details/120015593?spm=1001.2014.3001.5502)

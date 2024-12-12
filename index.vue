<template>
	<view>
		<view class="content">
			<view>
				<view class="station-view">
					<text class="station">站点查询</text>
				</view>
				<view class="pickers">				
					<view class="start-view">
						<picker mode="selector" :range="list" range-key="name" @change="onChange" filterable>
							<view class="picker">
								<text class="picker-text">
									{{selectedItem.name}}
								</text>
							</view>
						</picker>
					</view>
					  
					<view class="picker exchange-view width-view" @click="toExchange()">
						<text class="exchange">⇄</text>
					</view>		  
					  
					<view class="end-view">
						<picker mode="selector" :range="list" range-key="name" @change="onChangeFinal" filterable>
							<view class="picker">
								<text class="picker-text">
									{{selectedItemFinal.name}}
								</text>
							</view>
						</picker>
					</view>
					  
					<view class="picker price-view">
						<text class="picker-text price">¥{{price}}</text>
					</view>	
				</view>
			</view>
		</view>
		
		<view class="content pickers-group" v-if="finalLine.line_arr != undefined">
			<view>
				<view class="station-view">
					<text class="station">省钱推荐</text>
				</view>
				<view class="pickers" v-for="(item, index) in finalLine.line_arr" :key="index">				
					<view class="start-view">
						<view class="picker">
							<text class="picker-text">
								{{item.start}}
							</text>
						</view>
					</view>
					  
					<view class="picker width-direct-view">
						<text class="picker-text">→</text>
					</view>		  
					  
					<view class="end-view">
						<view class="picker">
							<text class="picker-text">
								{{item.end}}
							</text>
						</view>
					</view>	
				</view>
			</view>
					  
			<view class="picker price-view">
				<view>
					<text class="picker-text price">¥{{finalLine.all}}</text>
				</view>
				<view>
					<text class="picker-text save-price">(省¥{{save_price}})</text>
				</view>
			</view>
		</view>
		
		<view class="content" v-if="finalLine.line_arr != undefined">
			<view>
				<view class="station-view">
					<text class="station">温馨提示</text>
				</view>
				<view class="pickers" v-for="(item, index) in tips" :key="index">				
					<view>
						<view class="tips">
							<text class="picker-text save-price">
								{{item}}
							</text>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
	
	
	
</template>

<script>
	export default {
		data() {
			return {
				a:[
					'小金口',
					'云山',
					'西湖东',
					'陈江南',
					'常平南',
					'松山湖北',
					'西平西',
					'东莞西',
					'广州莲花山',
					'广州长隆',
					'番禺',
					'佛山西',
					'三水北',
					'大旺',
					'鼎湖东',
					'肇庆'
				],
				
				index_ago:0,
				index_after:17,
				
				is_reverse:0,
				
				a_reverse:[],
				
				b:{
				    '小金口云山' : 5,
				    '小金口西湖东' : 5,
				    '小金口陈江南' : 11,
				    '小金口常平南' : 33,
				    '小金口松山湖北' : 40,
				    '小金口西平西' : 51,
				    '小金口东莞西' : 59,
				    '小金口广州莲花山' : 68,
				    '小金口广州长隆' : 79,
				    '小金口番禺' : 82,
				    '小金口佛山西' : 104,
				    '小金口三水北' : 115,
				    '小金口大旺' : 125,
				    '小金口鼎湖东' : 136,
				    '小金口肇庆' : 153,
				
				    '云山西湖东' : 5,
				    '云山陈江南' : 9,
				    '云山常平南' : 30,
				    '云山松山湖北' : 37,
				    '云山西平西' : 48,
				    '云山东莞西' : 56,
				    '云山广州莲花山' : 65,
				    '云山广州长隆' : 76,
				    '云山番禺' : 80,
				    '云山佛山西' : 101,
				    '云山三水北' : 112,
				    '云山大旺' : 122,
				    '云山鼎湖东' : 134,
				    '云山肇庆' : 149,
				
				    '西湖东陈江南' : 7,
				    '西湖东常平南' : 28,
				    '西湖东松山湖北' : 36,
				    '西湖东西平西' : 46,
				    '西湖东东莞西' : 54,
				    '西湖东广州莲花山' : 63,
				    '西湖东广州长隆' : 74,
				    '西湖东番禺' : 78,
				    '西湖东佛山西' : 99,
				    '西湖东三水北' : 110,
				    '西湖东大旺' : 120,
				    '西湖东鼎湖东' : 132,
				    '西湖东肇庆' : 148,
				
				    '陈江南常平南' : 20,
				    '陈江南松山湖北' : 27,
				    '陈江南西平西' : 38,
				    '陈江南东莞西' : 46,
				    '陈江南广州莲花山' : 55,
				    '陈江南广州长隆' : 66,
				    '陈江南番禺' : 70,
				    '陈江南佛山西' : 91,
				    '陈江南三水北' : 102,
				    '陈江南大旺' : 113,
				    '陈江南鼎湖东' : 124,
				    '陈江南肇庆' : 140,
				
				    '常平南松山湖北' : 6,
				    '常平南西平西' : 17,
				    '常平南东莞西' : 26,
				    '常平南广州莲花山' : 34,
				    '常平南广州长隆' : 45,
				    '常平南番禺' : 48,
				    '常平南佛山西' : 71,
				    '常平南三水北' : 81,
				    '常平南大旺' : 91,
				    '常平南鼎湖东' : 102,
				    '常平南肇庆' : 118,
				
				    '松山湖北西平西' : 9,
				    '松山湖北东莞西' : 18,
				    '松山湖北广州莲花山' : 27,
				    '松山湖北广州长隆' : 37,
				    '松山湖北番禺' : 41,
				    '松山湖北佛山西' : 63,
				    '松山湖北三水北' : 74,
				    '松山湖北大旺' : 84,
				    '松山湖北鼎湖东' : 95,
				    '松山湖北肇庆' : 111,
				
				    '西平西东莞西' : 8,
				    '西平西广州莲花山' : 17,
				    '西平西广州长隆' : 27,
				    '西平西番禺' : 30,
				    '西平西佛山西' : 53,
				    '西平西三水北' : 63,
				    '西平西大旺' : 73,
				    '西平西鼎湖东' : 85,
				    '西平西肇庆' : 100,
				
				    '东莞西广州莲花山' : 8,
				    '东莞西广州长隆' : 18,
				    '东莞西番禺' : 22,
				    '东莞西佛山西' : 45,
				    '东莞西三水北' : 55,
				    '东莞西大旺' : 65,
				    '东莞西鼎湖东' : 76,
				    '东莞西肇庆' : 92,
				
				    '广州莲花山广州长隆' : 9,
				    '广州莲花山番禺' : 13,
				    '广州莲花山佛山西' : 36,
				    '广州莲花山三水北' : 46,
				    '广州莲花山大旺' : 56,
				    '广州莲花山鼎湖东' : 67,
				    '广州莲花山肇庆' : 83,
				
				    '广州长隆番禺' : 5,
				    '广州长隆佛山西' : 25,
				    '广州长隆三水北' : 36,
				    '广州长隆大旺' : 45,
				    '广州长隆鼎湖东' : 57,
				    '广州长隆肇庆' : 72,
				
				    '番禺佛山西' : 21,
				    '番禺三水北' : 32,
				    '番禺大旺' : 42,
				    '番禺鼎湖东' : 54,
				    '番禺肇庆' : 69,
				
				    '佛山西三水北' : 9,
				    '佛山西大旺' : 20,
				    '佛山西鼎湖东' : 31,
				    '佛山西肇庆' : 47,
				
				    '三水北大旺' : 9,
				    '三水北鼎湖东' : 20,
				    '三水北肇庆' : 36,
				
				    '大旺鼎湖东' : 10,
				    '大旺肇庆' : 27,
				
				    '鼎湖东肇庆' : 15,
				},
				
				listNew:[],	
				listNewReverse:[],			
				lines:[],			
				finalLine:[],
				
				list: [],
				selectedItem: {name: '小金口'},					  
				selectedItemFinal: {name: '肇庆'},					  
				price: '153',				  
				save_price: '0',	  
				tips: [
					"1、购买同一班次,确保操作成功",
					"2、购买晚些班次,以防退票需求",
					"3、乘坐分段班次,无需中途出站",
					"4、购买任一车次,可以随时上车",
				],
			}
		},
		onLoad() {
			let that = this;
			this.a.forEach(function(av, ak) {
				that.list.push({ name: av });
				that.a_reverse.unshift(av);
			})
			
			for(var i = 0;i<4;i++){
				this.listNew.push(this.a);
				this.listNewReverse.push(that.a_reverse);
			}
			
			this.toCal();
		},
		onShow() {
			
		},
		methods: {
			toCal() {			
				this.isReverse();
				this.lines = [];
				this.finalLine = [];
			  
				if(this.selectedItem.name == this.selectedItemFinal.name){
					uni.showToast({
					  title: '起点和终点不能相同',
					  duration: 2000
					});
					return;
				}
				  
				let that = this;
				let listTmp = [];
				if(that.is_reverse == 1){
					listTmp = that.listNewReverse;
					that.price = that.b[that.selectedItemFinal.name+that.selectedItem.name];
				}else{
					listTmp = that.listNew;
					that.price = that.b[this.selectedItem.name+that.selectedItemFinal.name];
				}
				
				listTmp[0].forEach(function(v0, k0) {
					if(v0 == that.selectedItem.name){
						listTmp[1].forEach(function(v1, k1) {
							if(k1 > k0){
								listTmp[2].forEach(function(v2, k2) {
									if(k2 > k0 && k2 >= k1){										
										listTmp[3].forEach(function(v3, k3) {
											if(v3 == that.selectedItemFinal.name && k1 < k3 && k2 < k3){
												let line = '';
												let line_1 = '';
												let line_2 = '';
												let line_3 = '';
												let line_arr = [];
												
												if(v1 == v2){
													line = v0+'=>'+v1+'=>'+v3;
													line_1 = v0+'=>'+v1;
													line_2 = v1+'=>'+v3;
													line_arr.push({
														'start':v0,
														'end':v1,
													});
													line_arr.push({
														'start':v1,
														'end':v3,
													});
												}else{
													line = v0+'=>'+v1+'=>'+v2+'=>'+v3;
													line_1 = v0+'=>'+v1;
													line_2 = v1+'=>'+v2;
													line_3 = v2+'=>'+v3;
													
													line_arr.push({
														'start':v0,
														'end':v1,
													});													
													line_arr.push({
														'start':v1,
														'end':v2,
													});													
													line_arr.push({
														'start':v2,
														'end':v3,
													});
												}
													   
												let one = 0;
												let two = 0;
												let three = 0;
													   
												let v01 = '';
												let v02 = '';
												let v03 = '';
												v01 = v0 + v1;
												v02 = v1 + v2;
												v03 = v2 + v3;
												
												if(that.is_reverse == 1){
													v01 = v1 + v0;
													v02 = v2 + v1;
													v03 = v3 + v2;
												}else{
													v01 = v0 + v1;
													v02 = v1 + v2;
													v03 = v2 + v3;
												}

												if(that.b[v01] != undefined){
													one = that.b[v01];
												}

												if(that.b[v02] != undefined){
													two = that.b[v02];
												}

												if(that.b[v03] != undefined){
													three = that.b[v03];
												}
												  
												let all  = one + two + three;
												   
												that.lines.push({
													'line_1' : line_1,
													'line_2' : line_2,
													'line_3' : line_3,
													'line' : line,
													'one' : one,
													'two' : two,
													'three' : three,
													'all' : all,
													'line_arr' : line_arr,
												});
											}
										});
									}
								});
							}
						});
					}
				});	
						
				this.lines.forEach(function(line, le) {					
					if(that.price > line['all']){
						if(that.finalLine.length == 0){
							that.finalLine = line;
						}else{
							if(that.finalLine['all'] > line['all']){
								that.finalLine = line;
							}
						}
					}
				})
				
				 
				if(that.finalLine.line != undefined && that.price > that.finalLine.all){							
					that.save_price = that.price - that.finalLine.all;
				}
			},
			
			toExchange() {
				let str = '';
				str = this.selectedItem;
				this.selectedItem = this.selectedItemFinal;
				this.selectedItemFinal = str;
			  
			    this.toCal();
			},
			
			onChange(e) {
				const index = e.detail.value;
				this.index_ago = index;
				this.selectedItem = this.list[index];
			  
				this.toCal();
			},
			
			onChangeFinal(e) {			  
				const index = e.detail.value;
				this.index_after = index;
				this.selectedItemFinal = this.list[index];

				this.toCal();
			},
			
			isReverse() {
				let ago = 0;
				let after = 0;
				let is_reverse = 0;
				let that = this;
				this.a.forEach(function(av, ak) {
					if(that.selectedItem.name == av){
						ago = ak;
					}
					if(that.selectedItemFinal.name == av){
						after = ak;
					}
				})
				
				if(ago < after){
					this.is_reverse = 0;
				}else{
					this.is_reverse = 1;
				}
			}
			
		}
	}
</script>

<style>
	page{
		background-color: rgb(233 236 242);
	}
	
	.station-view{
		background-color: hsl(155.45deg 100% 95.69%);
		padding: 10rpx 10rpx 10rpx 10rpx;
		width: 100rpx;
		display: flex;
		justify-content: center;
		border-radius: 20rpx;
		margin: 0rpx 0rpx 0rpx 10rpx;
	}
	
	.station{
		color: hsl(155.88deg 100% 39.02%);
		font-size: 20rpx;
	}
	
	.pickers-group{
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
	}
	
	.pickers{
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
	}
	
	.tips{
		margin: 0rpx 0rpx 10rpx 60rpx;
	}
	
	.start-view{
		width: 240rpx;
	}
	
	.width-view{
		width: 10rpx;
	}
	
	.width-direct-view{
		width: 30rpx;
		margin-left: -4rpx;
	}
	
	.end-view{
		width: 240rpx;
	}
	
	.price-view{
		width: 100rpx;
	}
	
	.picker {
	  padding: 10px;
	  background-color: #fff;
	  text-align: center;
	}
	
	.picker-text{
		font-size: 40rpx;
		color: rgb(51 51 51);
	}
	
	.exchange-view{
		background-color: #ff8000;
		border-radius: 50%;
		height: 6rpx;
		width: 6rpx;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	
	.exchange{
		color: #fff;
	}
	
	.price{
		color: rgb(255 106 57);
	}
	
	.save-price{
		font-size: 30rpx;
		color: rgb(255 106 57);
	}
	
	.content {
/* 		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center; */
		margin: 20rpx 20rpx 20rpx 20rpx;
		padding: 10rpx 10rpx 10rpx 10rpx;
		border-radius: 20rpx;
		background-color: #fff;
	}
</style>

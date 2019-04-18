<template>
	<view class="container">
		<view class="article-box">
			<view class="article" v-for="(article,index) in articles" :key="index">
				<!-- 标题 -->
				<text class="article-title" @tap="gotoDetail(article.id)">{{article.title}}</text>
				<!-- 大于等于三张图片的显示方式 -->
				<view class="" v-if="article.imgs.length >= 3">
					<view class="thumbnail-box">
						<view class="thumbnail-item" v-for="(item,index1) in article.imgs" :key="index1">
							<image :src="item.imgUrl">
							</image>
						</view>
					</view>
				</view>
				<!-- 小于三种图片的显示方式 -->
				<view class="two" v-else-if="article.imgs.length>= 1">
					<view class="text-img-box">
						<view class="left">
							<text>{{article.content}}</text>
						</view>
						<view class="right">
							<image :src="article.imgs[0].imgUrl" class="img"></image>
						</view>
					</view>
				</view>
				<!-- 没有图片的显示方式 -->
				<view class="text-box" v-else>
					<text>{{ article.title }}</text>
				</view>
				<!-- 文章作者等信息 -->
				<view class="article-info">
					<image :src="article.avatar" class="avatar small"></image>
					<text class="info-text">{{ article.nickname }}</text>
					<text class="info-text">{{ article.createTime }}</text>
				</view>
			</view>
		</view>
		<navigator url="../write/write" hover-class="navigator-hover">
			<button class="circle-btn"><text class="icon-text">+</text></button>
		</navigator>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				articles: []
			};
		},
		onLoad: function() {
			this.getArticles();
		},
		onShow: function() {},
		onPullDownRefresh: function() {
			this.getArticles();
		},
		methods: {
			getArticles: function() {
				var _this = this;
				uni.request({
					url: this.apiServer + "/article/list",
					method: 'GET',
					header: {
						'content-type': 'application/x-www-form-urlencoded'
					},
					success: res => {
						_this.articles = res.data.data;
					},
					complete: function() {
						uni.stopPullDownRefresh();
					}
				});
			},
			gotoDetail: function(aId) {
				uni.navigateTo({
					url: '../article_detail/article_detail?aId=' + aId
				});
			},
			handleTime: function(date) {
				var d = new Date(date);
				var year = d.getFullYear();
				var month = d.getMonth() + 1;
				var day = d.getDate() < 10 ? '0' + d.getDate() : '' + d.getDate();
				var hour = d.getHours() < 10 ? '0' + d.getHours() : '' + d.getHours();
				var minutes = d.getMinutes() < 10 ? '0' + d.getMinutes() : '' + d.getMinutes();
				var seconds = d.getSeconds() < 10 ? '0' + d.getSeconds() : '' + d.getSeconds();
				return year + '-' + month + '-' + day + '-' + hour + ":" + minutes + ':' + seconds;
			},
			handleContent: function(msg) {
				content = content.replaceAll(/(\n)/g, "");
				content = content.replaceAll(/(\t)/g, "");
				content = content.replaceAll(/(\r)/g, "");
				content = content.replaceAll(/<\/?[^>]*>/g, "");
				content = content.replaceAll(/\s*/g, "");
				return content.substring(0, 50);

			}
		}
	};
</script>

<style>
	.content {
		text-align: center;
	}

	.logo {
		height: 200upx;
		width: 200upx;
		margin-top: 200upx;
	}

	.title {
		font-size: 36upx;
		color: #8f8f94;
	}

	.add-title {
		float: right;
		width: 15%;
		position: absolute;
		right: 0px;
		bottom: 0px;
	}

	.article {
		display: flex;
		flex-direction: column;
		border-bottom: 1upx solid #EEEEEE;
		margin-top: 15px;
		padding-bottom: 15px;

	}

	.article-title {
		font-weight: bold;
		font-size: 25px;
		/* margin-right: 85%; */
	}

	.text-img-box {
		display: flex;
		flex-direction: row;
	}

	.thumbnail-box {
		display: flex;
		flex-direction: row;
	}

	.thumbnail-item image {
		width: 220upx;
		height: 220upx;
		margin-left: 25upx;
	}

	.left {
		flex: 1 1 60%;
		display: -webkit-box;
		-webkit-box-orient: vertical;
		-webkit-line-clamp: 4;
		overflow: hidden;
	}

	.right {
		flex: 1 1 50%;
		width: 80%;
		height: 80%;
	}

	.right image {
		width: 60%;
		height: 200upx;
	}

	/* .article-info {
		color: grey;
		width: 100%;
		margin-left:180upx;
		margin-top: 10px;
	} */

	.avatar {
		width: 75upx;
		height: 75upx;
		border-radius: 50%;
	}

	.circle-btn {
		position: fixed;
		width: 45px;
		height: 45px;
		right: 10px;
		bottom: 70px;
		border-radius: 50%;
		background-color: #de533a;
		background: linear-gradient(40deg, #ffd86f, #fc6262);
		box-shadow: 2px 5px 10px #D1D1D1;
		cursor: pointer;
		border: none;
		outline: none;
		display: flex;
		justify-content: center;
		align-items: center;

	}

	.icon-text {
		font-size: 20pt;
		color: #fff;
	}
</style>

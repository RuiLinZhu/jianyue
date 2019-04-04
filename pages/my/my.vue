<template>
	<view class="container">
		<view class="top">
			<view class="avatar-box">
				<image
					src="../../static/default.png"
					mode="scaleToFill"
					class="avatar"
					v-if="!storageData.login">
					</image>
				<image
					:src="avatar"
					mode="scaleToFill"
					class="avatar"
					v-if="storageData.login"
				></image>
			</view>
			<view class="info-box">
				<navigator url="../signin/signin" v-if="!storageData.login" class="btn">点击登录</navigator>
				<text v-if="storageData.login" class="nname">{{nickname }}</text>
				<navigator v-if="storageData.login" url="../setting/setting" class="set">个人设置</navigator>
			</view>
		</view>
		<view class="middle" v-if="storageData.login">
			<view class="information" v-for="(info,index) in infos" :key="index">
				<navigator class="un">{{info.number}}</navigator>
				<navigator class="text">{{info.text}}</navigator>
			</view>
		</view>
		<view class="bottom" v-if="storageData.login">
			<view class="crt" v-for="(article,index) in articles" :key="index">
				<navigator>{{article.text}}</navigator>
			</view>
		</view>
		
	</view>
</template>

<script>
var loginRes, _self;
export default {
	data() {
		var articlenum=10;
		var follownum=5;
		var msgnum=66;
		var creditnum=100;
		return {
			avatar:uni.getStorageSync('login_key').avatar ,
			nickname:uni.getStorageSync('login_key').nickname,
			storageData: {
			},
			infos:[
				{
				"number":articlenum,
				"text":"文章"
			    },
				{
				"number":follownum,
				"text":"关注"
				},
				{
				"number":msgnum,
				"text":"消息"
				},{
				"number":creditnum,
				"text":"积分"
			    }
			],
			articles:[
				{
					"text":"第一篇文章"
				},
				{
					"text":"第二篇文章"
				},
				{
					"text":"第三篇文章"
				},
				{
					"text":"第四篇文章"
				}
			]
		};
	},
	onLoad: function() {},
	onShow: function() {
		var _this = this;
		const loginKey = uni.getStorageSync('login_key');
		if (loginKey) {
			// console.log(loginKey);
			this.storageData = {
				login: loginKey.login,
				nickname: loginKey.nickname,
				avatar: loginKey.avatar
			};
		} else {
			this.storageData = {
				login: false
			};
		}
		uni.request({
			url: 'http://localhost:8080/api/user/' + uni.getStorageSync('login_key').userId,
			method: 'GET',
			header: { 'content-type': 'application/json' },
			success: res => {
				if (res.data.code === 0) {
					console.log(res.data.data.avatar+'————————————');
					_this.avatar = res.data.data.avatar;
					_this.nickname = res.data.data.nickname;
				}
			}
		});
	},
	methods: {
		
	}
};
</script>

<style scoped>
	.avatar-box{
		text-align: center;
		margin-top: 10px;
	}
	.info-box{
		display: flex;
		font-size: 13px;
	}
	.nname{
		padding-left: 130px;
		margin-right: 20px;
	}
	.set{
		color: rgb(26,173,25);
	}
	.middle{
		display: flex;
		margin-top: 10px;
		text-align: center;
	}
	.information{
	    width: 70%;
		/* margin-right: 10px; */
		text-align: center;
		/* margin-left: 10px; */
		border-right: 1px solid #8F8F94;
	}
	.un{
		font-family: "黑体";
		font-size: 25px;
	}
	.text{
		font-size: 13px;
	}
	.bottom{
		margin-top: 20px;
	}
	.crt{
		margin-bottom: 30px;
		border-top: 1px solid #8F8F94;
		/* border-left-color: #8F8F94;
		border-left-width: 2px; */
	}
	.btn{
		margin-top: 20px;
		margin-left: 150px;
		color: rgb(26,173,25);
	}
</style>
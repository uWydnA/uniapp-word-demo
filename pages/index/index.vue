<template>
	<view class="content" v-bind:style="{backgroundImage:'url(' +'http://114.67.65.56:8080/' + data.image + ')'}">
		<view class="mask">
		</view>
		<view class="cont" >
			<div class="bp3-button-group bp3-large bp3-minimal float-box">
				<button type="button" class="bp3-button searchBtn" @click='searchBtn'>
					<span icon="search-text" class="bp3-icon bp3-icon-search-text">
						<svg data-icon="search-text" width="16" height="16" viewBox="0 0 16 16">
							<desc>search-text</desc>
							<path d="M9 4H5c-.55 0-1 .45-1 1s.45 1 1 1h1v3c0 .55.45 1 1 1s1-.45 1-1V6h1c.55 0 1-.45 1-1s-.45-1-1-1zm6.56 9.44l-2.67-2.67C13.59 9.68 14 8.39 14 7c0-3.87-3.13-7-7-7S0 3.13 0 7s3.13 7 7 7c1.39 0 2.68-.41 3.77-1.11l2.67 2.67a1.498 1.498 0 102.12-2.12zM7 12c-2.76 0-5-2.24-5-5s2.24-5 5-5 5 2.24 5 5-2.24 5-5 5z"
							 fill-rule="evenodd"></path>
						</svg></span></button>
				<button type="button" class="bp3-button skipBtn" @click='skipBtn'>
					<span icon="chevron-right" class="bp3-icon bp3-icon-chevron-right">
						<svg data-icon="chevron-right" width="16" height="16" viewBox="0 0 16 16">
							<desc>chevron-right</desc>
							<path d="M10.71 7.29l-4-4a1.003 1.003 0 00-1.42 1.42L8.59 8 5.3 11.29c-.19.18-.3.43-.3.71a1.003 1.003 0 001.71.71l4-4c.18-.18.29-.43.29-.71 0-.28-.11-.53-.29-.71z"
							 fill-rule="evenodd"></path>
						</svg></span>
				</button>
			</div>
			<view class="typebox" v-if='data'>
				<view class="dist" v-if='!tapOk'>
					<img src="../../static/img/fox.png" alt="logo" class='logo'>
					<p>{{data.explain}}</p>
					<p>{{this.isSearchTap?data.word:data.word[0]+'______'+data.word[this.data.word.length-1]}}</p>
				</view>
				<view class="audio" v-if='tapOk'>
					<video v-show='false' controls="" autoplay="" name="media" :src="this.videoUrl">
					</video>
					<view class="newcont">
						{{data.word}}
					</view>
				</view>
				<div class="wordline" v-if='!tapOk'><span class="typeline">
						<div class="sc-bwzfXH ebDhOV">
							<div class='inputlist' v-if='data.word!==undefined' ref='inputlist'>
								<input v-for='(item,index) in data.word.length'  v-model="wordTap[index]" :focus="isWhichInput===index?true:false"
								 @focus="isFocus(index)" @keydown="changeValue($event,index)" type="text" maxlength="1" class="sc-bdVaJa ivAnOY"
								 autocomplete='off'>
							</div>
						</div>
					</span></div>
				<view class="explain" v-if='!tapOk'>all image from konachan.com</view>
			</view>
		</view>
	</view>
</template>

	
<script>
	import jsonData from '../../static/data.json'
	export default {
		data() {
			return {
				datalist: [],
				data:{},
				wordTap: [],
				isWhichInput: 0,
				trueFlag: 0,
				tapOk: false,
				videoUrl: '',
				isSearchTap: false,
				nowWordNum:0
			}
		},
		onLoad() {
			this.datalist = jsonData;
			this.data=this.datalist[this.getRandom(0,this.datalist.length)]
			console.log(this.data)
			this.nowWordNum = this.getRandom(0, this.datalist.length)
			this.data = this.datalist[this.nowWordNum]
		},
		methods: {
			getRandom(a, b) {
				return parseInt((Math.random() * (b - a) + a))
			},
			isFocus(index) {
				document.querySelectorAll('.ivAnOY').forEach((val, ind) => {
					if (index === ind) {
						val.className = 'uni-input-input sc-bdVaJa ivAnOY active'
					} else {
						val.className = 'uni-input-input sc-bdVaJa ivAnOY'
					}
				})
			},
			searchBtn() {
				this.isSearchTap=!this.isSearchTap
			},
			skipBtn() {
				this.isSkip()
			},
			isSkip(){
				let omask = document.querySelector('.mask')
				omask.style.backgroundColor = `rgba(0, 0, 0,0)`
				this.tapOk = true;
				this.videoUrl = `http://dict.youdao.com/dictvoice?audio=${this.data.word}`
				setTimeout(() => {
					this.nowWordNum = this.getRandom(0, this.datalist.length)
					this.data = this.datalist[this.nowWordNum];
					omask.style.backgroundColor = `rgba(0, 0, 0,.9)`
					this.trueFlag = 0;
					this.tapOk = false
					this.wordTap = [];
					this.isWhichInput = 0;
					this.isSearchTap = false
				}, 2000)
			},
			changeValue(eve, index) {
				setTimeout(() => {
					if (this.wordTap.length >= index + 1 & this.wordTap.length !== 0 && this.wordTap[index] !== '') {
						this.isWhichInput = index + 1;
					}
					if (this.wordTap[index] === '' || this.wordTap[index] === undefined) {
						this.isWhichInput = index - 1;
						this.wordTap[this.isWhichInput] = '';
					}
					let omask = document.querySelector('.mask')
					this.wordTap.forEach((val, index) => {
						if (val === this.data.word[index]) {
							this.trueFlag = index + 1

						} else {
							this.trueFlag = index;
						}
					})
					let num = 1 - (this.trueFlag / this.data.word.length)
					omask.style.backgroundColor = `rgba(0, 0, 0, ${num})`

					if (this.wordTap.join('') === '') {
						omask.style.backgroundColor = `rgba(0, 0, 0,.9)`
					}
					if (this.data.word === this.wordTap.join('')) {
						this.isSkip()
					}
				}, 1)
			}
		}
	}
</script>

<style lang="scss" scoped>
	.float-box {
		position: absolute;
		left: 20px;
		top: 20px;
		display: flex;
	}

	.bp3-button-group.bp3-minimal .bp3-button {
		-webkit-box-shadow: none;
		box-shadow: none;
		background: none;
	}

	.bp3-button-group .bp3-button {
		-webkit-box-flex: 0;
		-ms-flex: 0 0 auto;
		flex: 0 0 auto;
		position: relative;
		z-index: 4;
	}

	.bp3-button.bp3-large,
	.bp3-large .bp3-button {
		min-width: 40px;
		min-height: 40px;
		padding: 5px 15px;
		font-size: 16px;
	}

	.bp3-button .bp3-icon:first-child:last-child,
	.bp3-button .bp3-spinner+.bp3-icon:last-child {
		margin: 0 -7px;
	}

	.bp3-icon>svg:not([fill]) {
		fill: currentColor;
	}

	.float-box svg {
		color: white;
	}

	.content {
		height: 100vh;
		width: 100vw;
		overflow: hidden;
		background-size: cover;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.mask {
		width: 100%;
		height: 100%;
		background-color: rgba(0, 0, 0, 0.9);
		position: fixed;
		left: 0px;
		right: 0px;
		top: 0px;
		bottom: 0px;
		z-index: 2;
	}

	.test {
		z-index: 999;
	}

	.cont {
		z-index: 9999;
	}

	.typebox {
		z-index: 9999;
		min-width: 100%;
		max-width: 100%;
		height: 100vh;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;

		.dist {
			color: white;
			font-size: 24px;
			text-align: center;
			padding-left: 40px;
			padding-right: 40px;
			padding-top: 40px;

			.logo {
				max-width: 300px;
				max-height: 300px;
			}

			p {
				margin-top: 0;
				margin-bottom: 10px;
			}
		}

		.wordline {
			padding-left: 40px;
			padding-right: 40px;
			padding-bottom: 40px;
			display: flex;
			align-items: center;
			justify-content: space-between;
			flex-direction: row;
			color: white;

			.typeline {
				flex: 1;
			}

			.typeline>div {
				width: auto;
				margin-top: auto;
			}
		}
	}

	.ivAnOY {
		background-color: #fff;
		display: inline-block;
		-webkit-box-pack: justify;
		justify-content: space-between;
		width: 45px;
		height: 45px;
		text-align: center;
		font-size: 34px;
		font-family: -apple-system, system-ui, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", "Arial sans-serif";
		margin: 5px;
		border-radius: 5px;
		color: #000;
	}

	.active {
		outline: rgba(19, 124, 189, 0.6) auto 2px;
		outline-offset: 2px;
		-moz-outline-radius: 6px !important;
	}

	.explain {
		color: #ccc;
		margin-bottom: 15px;
		font-size: 2upx;
	}

	.newcont {
		font-family: Georgia, Times, 'Times New Roman', serif;
		color: rgba(255, 255, 255, 0.99);
		text-shadow: 2px 2px 2px rgba(0, 0, 0, 0.6);
		font-size: 10vw;
		letter-spacing: 10px;
	}
	uni-button:after{
		border:0 !important
	}
</style>

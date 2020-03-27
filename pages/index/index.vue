<template>
	<view class="content">
		<view class="mask">
		</view>
		<view class="cont">
			<view class="btn"></view>
			<view class="typebox">
				<view class="dist" v-if='!this.tapOk'>
					<img src="../../static/img/fox.png" alt="logo" class='logo'>
					<p>{{data.content}}</p>
					<p>{{data.word[0]}}____{{data.word[data.word.length-1]}}</p>
				</view>
				<view class="audio" v-if='this.tapOk'>
					<video v-show='false' controls="" autoplay="" name="media" :src="this.videoUrl">
					</video>
					<view class="newcont">
						{{this.data.word}}
					</view>
				</view>
				<div class="wordline" v-if='!this.tapOk'><span class="typeline">
						<div class="sc-bwzfXH ebDhOV">
							<div class='inputlist' v-if='data.word!==undefined' ref='inputlist'>
								<input v-for='(item,index) in data.word.length' v-model="wordTap[index]" :focus="isWhichInput===index?true:false"
								 @focus="isFocus(index)" @keydown="changeValue($event,index)" type="text" maxlength="1" class="sc-bdVaJa ivAnOY"
								 autocomplete='off'>
							</div>
						</div>
					</span></div>
				<view class="explain" v-if='!this.tapOk'>all image from konachan.com</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				datalist: [{
						word: 'not',
						content: 'ad.不，没有'
					},
					{
						word: 'marriage',
						content: 'n.结婚，婚姻；婚礼'
					}
				],
				data: {
					word: 'not',
					content: 'ad.不，没有'
				},
				wordTap: [],
				isWhichInput: 0,
				trueFlag: 0,
				tapOk: false,
				videoUrl: ''
			}
		},
		onLoad() {

		},
		methods: {
			isFocus(index) {
				document.querySelectorAll('.ivAnOY').forEach((val, ind) => {
					if (index === ind) {
						val.className = 'uni-input-input sc-bdVaJa ivAnOY active'
					} else {
						val.className = 'uni-input-input sc-bdVaJa ivAnOY'
					}
				})
				let omask = document.querySelector('.mask')
				if ((this.data.word).includes(this.wordTap.join('')) && this.wordTap.length > 0) {
					let num = 1 - (++this.trueFlag / this.data.word.length)
					omask.style.backgroundColor = `rgba(0, 0, 0, ${num})`
				} else if ((this.data.word).includes(this.wordTap.join('')) === false && this.wordTap.length > 0) {
					let num = 1 - (--this.trueFlag / this.data.word.length)
					omask.style.backgroundColor = `rgba(0, 0, 0, ${num})`
				} else {
					omask.style.backgroundColor = `rgba(0, 0, 0,.9)`
				}


			},
			changeValue(eve, index) {
				setTimeout(() => {
					if (this.wordTap.length >= index & this.wordTap.length !== 0) {
						this.isWhichInput = index + 1;
					}
					if (this.data.word === this.wordTap.join('')) {
						document.querySelector('.mask').style.backgroundColor = `rgba(0, 0, 0,0)`
						this.tapOk = true;
						this.videoUrl = `http://dict.youdao.com/dictvoice?audio=${this.data.word}`
					}
				}, 10)
			}
		}
	}
</script>

<style lang="scss" scoped>
	.content {
		background-image: url('../../static/img/56352254.jpg');
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
</style>

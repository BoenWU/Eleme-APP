<template>
<div>
	<div class="shopcart">
		<div class="content" @click="toggleList">
			<div class="content-left">
				<div class="logo-wrapper">
					<div class="logo" :class="{'highlight':totalCount}">
						<i class="icon-shopping_cart"  :class="{'highlight':totalCount}"></i>
					</div>
					<div class="num" v-show="totalCount">{{totalCount}}</div>
				</div>
				<div class="price" :class="{'highlight':totalPrice}">￥{{totalPrice}}</div>
				<div class="desc">另需配送费￥{{deliveryPrice}}元</div>
			</div>
			<div class="content-right" @click.stop.prevent="pay">
				<div class="pay" :class="payClass">
					{{payDesc}}
				</div>
			</div>
		</div>
		<div class="ball-cotainer">	
			<transition name="drop" @before-enter="beforeEnter" @enter="enter" @after-enter="afterEnter">
			<div class="ball" v-for="(ball,index) in balls" v-show="ball.show">
				<div class="inner inner-hook"></div>
			</div>
			</transition>
		</div>
		<transition name=fold>
		<div class="shopcart-list" v-show="listShow">
			<div class="list-header">
				<h1 class="title">购物车</h1>
				<span class="delete" @click="empty">清空</span>
			</div>
			<div class="list-content" ref="list">
				<ul>
					<li class="food" v-for="food in selectFoods">
						<span class="name">{{food.name}}</span>
						<div class="price">
							<span>￥{{food.price*food.count}}</span>
						</div>
						<div class="cartcontrol-wrapper">
							<cartcontrol :food="food"></cartcontrol>
						</div>
					</li>
				</ul>
			</div>
		</div>
		</transition>
	</div>
	<transition name="fade">
	<div class="list-mask" @click="hideList" v-show="listShow"></div>
	</transition>
</div>
</template>

<script type="text/ecmascript-6">
import Bscroll from 'better-scroll'
import cartcontrol from '../cartcontrol/cartcontrol'
	export default {
		props: {
			selectFoods: {
				type: Array,
				default () {
					return [{
						price: 10,
						count: 1
					}]
				}
			},
			deliveryPrice: {
				type: Number,
				default: 0
			},
			minPrice: {
				type: Number,
				default: 0
			}
		},
		data () {
		return {
			balls : [{
				show:false
			},
			{
				show:false
			},
			{
				show:false
			},
			{
				show:false
			},
			{
				show:false
			}],
			dropBall: [],
			fold: true
			}
		},
		computed: {
			totalPrice () {
				let total = 0
				this.selectFoods.forEach((food) => {
					total += food.price * food.count
				})
				return total
			},
			totalCount () {
				let count = 0
				this.selectFoods.forEach((food) => {
					count += food.count
				})
				return count
			},
			payDesc () {
				if (this.totalPrice === 0) {
					return ('￥' + this.minPrice + '元起送')
				} else if (this.totalPrice < this.minPrice) {
					let diff = this.minPrice - this.totalPrice
					return ('还差￥' + diff + '元起送')
				} else {
					return '去结算'
				}
			},
			payClass () {
				if (this.totalPrice < this.minPrice) {
					return 'not-enough'
				} else {
					return 'enough'
				}
			},
			listShow () {
				if (!this.totalCount) {
					this.fold = true
					return false
				}
				let show = !this.fold
				if (show) {
				this.$nextTick(() => {
					if (!this.scroll) {
						this.scroll = new Bscroll(this.$refs.list, {
							click:true
						})
					} else {
						this.scroll.refresh()
					}
				})
				}
				return show
			}
		},
		methods: {
			drop (el) {
			console.log(el)
				for (let i = 0; i < this.balls.length; i++) {
					let ball = this.balls[i]
					if (!ball.show) {
						ball.show = true
						ball.el = el
						this.dropBall.push(ball)
						return
					}
				}
			},
			beforeEnter (el) {
				let count = this.balls.length
				while (count--) {
					let ball = this.balls[count]
					if (ball.show) {
						let rect = ball.el.getBoundingClientRect()
						let x = rect.left - 32
						let y = -(window.innerHeight - rect.top - 22)
						console.log(x, y)
						el.style.display = 'block'
						el.style.webkitTransform = 'translate3d(0,' + y + 'px,0)'
						el.style.transform = 'translate3d(0,' + y + 'px,0)'
						let inner = el.getElementsByClassName('inner-hook')[0]
						inner.style.webkitTransform = 'translate3d(' + x + 'px,0,0)'
						inner.style.transform = 'translate3d(' + x + 'px,0,0)'
					}
				}
			},
			enter (el, done) {
				/* eslint-disable no-unused-vars */
				let rf = el.offsetHeight
				this.$nextTick(() => {
						el.style.webkitTransform = 'translate3d(0,0,0)'
						el.style.transform = 'translate3d(0,0,0)'
						let inner = el.getElementsByClassName('inner-hook')[0]
						inner.style.webkitTransform = 'translate3d(0,0,0)'
						inner.style.transform = 'translate3d(0,0,0)'
						done()
				})
			},
			afterEnter (el) {
				let ball = this.dropBall.shift()
				if (ball) {
				ball.show = false
				el.style.display = 'block'
				console.log(this.balls)
				}
			},
			toggleList () {
				if (!this.totalCount) {
				return
				}
				this.fold = !this.fold
			},
			empty () {
				this.selectFoods.forEach((food) => {
					food.count = 0
				})
			},
			hideList () {
				this.fold = true
			},
			pay () {
				if (this.totalPrice < this.minPrice) {
				return
				}
				window.alert('支付$' + this.totalPrice)
			}
		},
		components: {
			cartcontrol
		}

	}
</script>

<style lang="less" rel="stylesheet/less">
@import "../../common/less/mixin.less";
@import "../../common/less/base.less";
	.shopcart{
		position: fixed;
		left: 0;
		bottom:0;
		z-index: 50;
		width: 100%;
		height: 48px;
		.content{
			display: flex;
			background: #141d27;
			font-size: 0;
			color:rgba(255,255,255,.4);
			.content-left{
				flex:1;
				.logo-wrapper{
					position:relative;
					display:inline-block;
					vertical-align: top;
					top:-10px;
					margin: 0 12px;
					padding:6px;
					width:56px;
					height: 56px;
					box-sizing:border-box;
					border-radius: 50%;
					background:#141d27;
					.logo{
						width:100%;
						height: 100%;
						border-radius: 50%;
						text-align: center;
						background:#2b343c;
						&.highlight{
							background:rgb(0,160,220)
						}
						.icon-shopping_cart{
							font-size: 24px;
							color:#80858a;
							line-height: 44px;
							&.highlight{
								color:#fff;
							}
						}
					}
					.num{
						position:absolute;
						top: 0;
						right: 0;
						width: 24px;
						height: 16px;
						line-height:16px;
						text-align:center;
						border-radius:16px;
						font-size: 9px;
						font-weight: 700;
						color:#fff;
						background:rgb(240,20,20);
						box-shadow: 0 4px 8px 0 rgba(0,0,0,.4);
					}
				}
				.price{
					display:inline-block;
					vertical-align:top;
					line-height:24px;
					margin-top:12px;
					padding-right: 12px;
					box-sizing: border-box;
					border-right: 1px solid rgba(255,255,255,.1);
					font-size: 16px;
					font-weight: 700;
					&.highlight{
						color:#fff;
					}
				}
				.desc{
					display:inline-block;
					vertical-align:top;
					margin:12px 0 0 12px;
					line-height:24px;
					font-size: 10px;
				}
			}
			.content-right{
				flex:0 0 105px;
				width:105px;
				.pay{
					height: 48px;
					line-height:48px;
					text-align: center;
					font-size: 12px;
					background:#2b333b;
					&.not-enough{
						background:#2b333b;
					}
					&.enough{
						background:#00b43c;
						color:#fff;
					}
				}
			}
		}
		.ball-cotainer{
			.ball{
				position:fixed;
				left:32px;
				bottom:22px;
				z-index: 200;
				transition: all 0.4s cubic-bezier(0.49,-0.29,0.75,0.41);
				.inner{
					width: 16px;
					height: 16px;
					border-radius: 50%;
					background:rgb(0,160,220);
					transition: all 0.4s;
				}
			}
		}
		.shopcart-list{
			position:absolute;
			top:0;
			left:0;
			z-index:-1;
			width: 100%;
			transform:translate3d(0,-100%,0);
			transition:all 0.5s;
			transform:translate3d(0,-100%,0);
			&.food-enter,&.fold-leave-active{
				transform:translate3d(0,0,0);
			}
			.list-header{
				height: 40px;
				line-height:40px;
				padding:0 18px;
				background:#f3f5f7;
				border-bottom: 1px solid rgba(7,17,27,.1);
				.title{
					float:left;
					color:rgb(7,17,27);
				}
				.delete{
					float:right;
					font-size: 12px;
					color:rgb(0,160,220);
				}
			}
				.list-content{
					padding:0 18px;
					max-height: 217px;
					overflow: hidden;
					background:#fff;
					.food{
						position:relative;
						padding:12px 0;
						.border1(rgba(7,17,27,.1));
						.name{
							line-height:24px;
							font-size: 14px;
							color:rgb(7,17,27);
						}
						.price{
							position:absolute;
							right:90px;
							bottom:12px;
							line-height:24px;
							font-size: 14px;
							font-weight: 700;
							color:rgb(240,20,20);
						}
						.cartcontrol-wrapper{
							position:absolute;
							right:0;
							bottom:6px;
						}
					}
				}
		}
	}
	.list-mask{
		position:fixed;
		top:0;
		left:0;
		width: 100%;
		height: 100%;
		z-index:40;
		transition:all 0.5s;
		backdrop-filter:blur(10px);
		background:rgba(7,17,27,0.6);
		&.fade-enter,&.fade-leave-active{
			opacity: 0;
			background:transparent;
		}
	}
</style>
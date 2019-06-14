<template>
    <div>
    	<head-top signin-up='msite'>
    		<router-link :to="'/search/geohash'" class="link_search" slot="search">
	    		<svg width="100%" height="100%" xmlns="http://www.w3.org/2000/svg" version="1.1">
	    			<circle cx="8" cy="8" r="7" stroke="rgb(255,255,255)" stroke-width="1" fill="none"/>
	    			<line x1="14" y1="14" x2="20" y2="20" style="stroke:rgb(255,255,255);stroke-width:2"/>
	    		</svg>
    		</router-link>
			<div><span class="title_text ellipsis">排行</span></div>
			<!-- <router-link to="/home" slot="msite-title" class="msite_title">
				<span class="title_text ellipsis">{{msietTitle}}</span>
			</router-link> -->
			<router-link to="/home" slot="msite-title" class="msite_title">
				<span class="title_text ellipsis">观点排行</span>
			</router-link>
    	</head-top>
		<nav class="nav-box">
               <section class="nav-box-top">
                   <div v-for="(o,index) in navList" :key="index" :class="{'active-div':activeNav==index}" @click="chengeNav(index)"> {{o}}</div>
                   <!-- <div class="active-div">点击榜</div>
                   <div>评分榜</div> -->
               </section>
               <section class="nav-box-bottom">
                   <ul class="silder-ul">
                       <li class="active-li">课程</li>
                       <li v-for="(item,index) in navSecList" :key="index">{{item}}</li>
                       <!-- <li>课程</li>
                       <li>课程</li>
                       <li>课程</li>
                       <li>课程</li>
                       <li>课程</li> -->
                   </ul>
               </section>
        </nav>
		<div class="search_none" v-if="emptyResult">很抱歉！无搜索结果</div>
    	<!-- <nav class="msite_nav">
    		<div class="swiper-container" v-if="foodTypes.length">
		        <div class="swiper-wrapper">
		            <div class="swiper-slide food_types_container" v-for="(item, index) in foodTypes" :key="index">
	            		<router-link :to="{path: '/food', query: {geohash, title: foodItem.title, restaurant_category_id: getCategoryId(foodItem.link)}}" v-for="foodItem in item" :key="foodItem.id" class="link_to_food">
	            			<figure>
	            				<img :src="imgBaseUrl + foodItem.image_url">
	            				<figcaption>{{foodItem.title}}</figcaption>
	            			</figure>
	            		</router-link>
		            </div>
		        </div>
		        <div class="swiper-pagination"></div>
		    </div>
		    <img src="../../images/fl.svg" class="fl_back animation_opactiy" v-else>
    	</nav> -->
    	<div class="shop_list_container">
	    	<!-- <header class="shop_header">
	    		<svg class="shop_icon">
	    			<use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#shop"></use>
	    		</svg>
	    		<span class="shop_header_title">附近商家</span>
	    	</header> -->
	    	<rangking-list v-if="hasGetData" :geohash="geohash"></rangking-list>
    	</div>
    	<foot-guide></foot-guide>
    </div>    
</template>

<script>
import {mapMutations} from 'vuex'
// import {imgBaseUrl} from 'src/config/env'
import headTop from 'src/components/header/head'
import footGuide from 'src/components/footer/footGuide'
import rangkingList from 'src/components/common/rankinglist'
import {msiteAdress, msiteFoodTypes, cityGuess} from 'src/service/getData'
import 'src/plugins/swiper.min.js'
import 'src/style/swiper.min.css'

export default {
	data(){
        return {
        	geohash: '', // city页面传递过来的地址geohash
            msietTitle: '请选择地址...', // msiet页面头部标题
            foodTypes: [], // 食品分类列表
            hasGetData: false, //是否已经获取地理位置数据，成功之后再获取商铺列表信息
            imgBaseUrl: 'https://fuss10.elemecdn.com', //图片域名地址
            navList:["点击榜","评分榜"],
            navSecList:["舞蹈","明星","体育","旅游","教育","同城","游戏","骑行"],//二级导航,
            activeNav:0,
        }
    },
    async beforeMount(){
		if (!this.$route.query.geohash) {
			const address = await cityGuess();
			this.geohash = address.latitude + ',' + address.longitude;
		}else{
			this.geohash = this.$route.query.geohash
		}
		//保存geohash 到vuex
		this.SAVE_GEOHASH(this.geohash);
    	//获取位置信息
    	let res = await msiteAdress(this.geohash);
    	this.msietTitle = res.name;
    	// 记录当前经度纬度
    	this.RECORD_ADDRESS(res);

    	this.hasGetData = true;
    },
    mounted(){
        //获取导航食品类型列表
       	msiteFoodTypes(this.geohash).then(res => {
       		let resLength = res.length;
       		let resArr = [...res]; // 返回一个新的数组
       		let foodArr = [];
    		for (let i = 0, j = 0; i < resLength; i += 8, j++) {
    			foodArr[j] = resArr.splice(0, 8);
    		}
    		this.foodTypes = foodArr;
        }).then(() => {
        	//初始化swiper
        	new Swiper('.swiper-container', {
		        pagination: '.swiper-pagination',
		        loop: true
		    });
        })
    },
    components: {
    	headTop,
    	rangkingList,
    	footGuide,
    },
    computed: {

    },
    methods: {
    	...mapMutations([
    		'RECORD_ADDRESS', 'SAVE_GEOHASH'
    	]),
    	// 解码url地址，求去restaurant_category_id值
    	getCategoryId(url){
    		let urlData = decodeURIComponent(url.split('=')[1].replace('&target_name',''));
    		if (/restaurant_category_id/gi.test(urlData)) {
    			return JSON.parse(urlData).restaurant_category_id.id
    		}else{
    			return ''
    		}
        },
        chengeNav(index){
            this.activeNav=index;
        }
    },
    watch: {

    }
}

</script>

<style lang="scss" scoped>
    @import 'src/style/mixin';
	.link_search{
		left: .8rem;
		@include wh(.9rem, .9rem);
		@include ct;
	}
	.msite_title{
		@include center;
        width: 50%;
        color: #fff;
        text-align: center;
        margin-left: -0.5rem;
        .title_text{
            @include sc(0.8rem, #fff);
            text-align: center;
            display: block;
        }
	}
	.search_form{
        background-color:$blue;
        padding: 0.5rem;
		display: flex;
		margin-top: 1.95rem;
        input{
            height: 1.5rem;
        }
        .search_input{
            flex: 4;
            border: 0.025rem solid $bc;
            // @include sc(0.65rem, #fff);
			border-radius: 1rem;
            background-color: $blue;;
			padding: 0 0.25rem;
			padding-left: 2.5rem;
        }
        .search_submit{
            flex: 1;
            border: 0.025rem solid $blue;
            margin-left: .2rem;
            @include sc(0.65rem, #fff);
            border-radius: 0.125rem;
            background-color: $blue;
            font-weight: bold;
            padding: 0 0.25rem;
        }
    }
	.msite_nav{
		padding-top: 2.1rem;
		background-color: #fff;
		border-bottom: 0.025rem solid $bc;
		height: 10.6rem;
		.swiper-container{
			@include wh(100%, auto);
			padding-bottom: 0.6rem;
			.swiper-pagination{
				bottom: 0.2rem;
			}
		}
		.fl_back{
			@include wh(100%, 100%);
		}
	}
	.food_types_container{
		display:flex;
		flex-wrap: wrap;
		.link_to_food{
			width: 25%;
			padding: 0.3rem 0rem;
			@include fj(center);
			figure{
				img{
					margin-bottom: 0.3rem;
					@include wh(1.8rem, 1.8rem);
				}
				figcaption{
					text-align: center;
					@include sc(0.55rem, #666);
				}
			}
		}
	}
	.shop_list_container{
		margin-top: 7rem;
		border-top: 0.025rem solid $bc;
		background-color: #fff;
		.shop_header{
			.shop_icon{
				fill: #999;
				margin-left: 0.6rem;
				vertical-align: middle;
				@include wh(0.6rem, 0.6rem);
			}
			.shop_header_title{
				color: #999;
				@include font(0.55rem, 1.6rem);
			}
		}
	}
	.svg-circle{
		width: 0.8rem;
		height: 0.8rem;
		position: absolute;
		margin-left: 1.5rem;
		top: 2.75rem;
	}
	.circle-red{
		font-size: 0.55rem;
		margin: 0.25rem auto;
		width: 1rem;
		height: 1rem;
		line-height: 1rem;
		background-color: red;
		text-align: center;
		border-radius: 50%;
		margin-left: 0.25rem;
		position: absolute;
		span{
			color: #fff;
    		font-weight: 600;
		}
	}
	.circle-ori{
		font-size: 0.55rem;
		margin: 0.25rem auto;
		width: 1rem;
		height: 1rem;
		line-height: 1rem;
		background-color: red;
		text-align: center;
		border-radius: 50%;
		right: 2.8rem;
		position: absolute;
	span{
		color: #fff;
		font-weight: 600;
		}
    }
	.search_fx{
	    height: 1.5rem;
    width: 1rem;

  
    display: flex;
    vertical-align: middle;
   
    justify-content: center; 
    padding: 0.25rem 0rem  0.25rem  0.25rem ;
		img{
			width: 1rem;
			height: 1rem;
			padding: 0rem 0.6rem;
		}
	}
    .nav-box{

        margin-top: -5.1rem;
        background-color:$blue;
        position: fixed;
        width: 100%;
        .nav-box-top{
            display: flex;
            div{
                margin: 1rem 2rem;
                color: #fff;
                font-size: .8rem;
                padding: .2rem;
                text-align: center;
                -webkit-flex: auto;
                -ms-flex: auto;
                flex: auto;
            }
            .active-div{
                border: 0.02rem solid #d2b9b9;
                border-radius: 3rem;
                box-shadow: inset 0px 1px 19px 1px #ada7a7;
            }

        }
        .nav-box-bottom{
            background-color: #8e3662;
            ul{
                overflow-x: scroll;
                white-space: nowrap;
                li{
                    display: inline-block;
                    padding: .25rem .8rem;
                    color: #fff;
                    font-size: .7rem;
                }
                .active-li{
                    border-bottom: .05rem solid #fff;
                }
            }
        }
    }
</style>

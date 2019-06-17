 <template>
  <div class="page">
        <head-top head-title="我的点包" go-back='true'></head-top>
        <section class="content_container">
            <section class="content">
                <header class="content_header">
                    <span class="content_title_style">总资产</span>
                    <!-- <section class="contetn_description">
                        <img src="../../images/description.png" height="24" width="24">
                        <router-link to="/balance/detail" class="content_title_style">提现说明</router-link>
                    </section> -->
                </header>
                <p class="content_num">
                    <span>100.00</span>
                    <span>元</span>
                </p>
            </section>
             <section class="content content-box">
                <div class="content_num content_num-div b-right">
                    <span>可用点数</span><br>
                    <span>100.00</span>
                </div>
                <div class="content_num content_num-div">
                    <span>已购点数</span><br>
                    <span>100.00</span>
                </div>
            </section>
        </section>
         <section class="profile-1reTe">
            <!-- <router-link to='/' class="myorder">
                <aside>
                    <svg fill="#ffc636">
                        <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#vip"></use>
                    </svg>
                </aside>
                <div class="myorder-div">
                    <span>余额收入支出详情</span>
                    <span class="myorder-divsvg">
                        <svg fill="#bbb">
                            <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#arrow-right"></use>
                        </svg>
                    </span>
                </div>
            </router-link> -->
             <router-link to='/' class="myorder">
                <aside>
                    <svg fill="#ffc636">
                        <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#vip"></use>
                    </svg>
                </aside>
                <div class="myorder-div">
                    <span>点包明细</span>
                    <span class="myorder-divsvg">
                        <svg fill="#bbb">
                            <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#arrow-right"></use>
                        </svg>
                    </span>
                </div>
            </router-link>
        </section>
        <section class="promit_button-box">
            <!-- <div class="promit_button">提现</div> -->
            <div class="promit_button">我要充值</div>
        </section>
        <!-- <p class="deal_detail">交易明细</p>
        <div class="no_log">
            <img src="../../images/no-log.png">
            <p>暂无明细记录</p>
        </div>
        <alert-tip v-if="showAlert" @closeTip="showAlert = false" :alertText="alertText"></alert-tip>
        <transition name="router-slid" mode="out-in">
            <router-view></router-view>
        </transition> -->
    </div>
</template>

<script>
    import headTop from 'src/components/header/head'
    import alertTip from 'src/components/common/alertTip'
    
    export default {
      data(){
            return{
                showAlert: false,
                alertText: null,
            }
        },
        mounted(){
          
        },
        components: {
            headTop,
            alertTip,
        },
        computed: {
           
        },
        methods: {
            
        }
    }
</script>
  
<style lang="scss" scoped>
    @import 'src/style/mixin';
  
    .page{
        padding-top: 1.95rem;
        p, span{
            text-align:center;
            font-family: Helvetica Neue,Tahoma,Arial;
        }
    }
    .promit_button-box{
        position:absolute;
        bottom:2rem;
        width:100%
    }
    .promit_button{
        @include wh(80%, 2rem);
        @include sc(.8rem, #fff);
        border-radius: 1rem;
        line-height: 2rem;
        margin:.5rem auto;
        text-align: center;
        background-color:$blue;
        background: linear-gradient(to right,#dc87b1,#a64274);
    }
    .content_container{
        padding: .3rem;
        // background-color: $blue;
        .content{
            padding: .4rem;
            // background-color: #fff;
            border-radius: .15rem;
            .content_header{
                @include fj;
                font-size: .55rem;
                .contetn_description{
                    display: flex;
                    align-items: center;
                    img{
                        @include wh(.6rem, .6rem);
                        margin-right: .2rem;
                    }
                    .content_title_style{
                        color: $blue;
                    }
                }
                .content_title_style{
                    color: #666;
                }
            }
            .content_num{
                span:nth-of-type(1){
                    @include sc(1.8rem, $blue);
                }
                span:nth-of-type(2){
                    @include sc(.7rem, #333);
                }
            }
            .promit_button{
                @include wh(100%, 2rem);
                @include sc(.8rem, #fff);
                border-radius: 0.15rem;
                line-height: 2rem;
                margin-top: 1rem;
                text-align: center;
                background-color:$blue;
            }
        }
    }
    .deal_detail{
        @include sc(.6rem, #999);
        line-height: 2rem;
        padding-left: .5rem;
    }
    .no_log{
        text-align: center;
        margin-top: 1rem;
        img{
            @include wh(8rem, 5rem);
        }
        p{
            margin-top: .5rem;
            @include sc(.7rem, #666);
        }
    }
    .router-slid-enter-active, .router-slid-leave-active {
        transition: all .4s;
    }
    .router-slid-enter, .router-slid-leave-active {
        transform: translate3d(2rem, 0, 0);
        opacity: 0;
    }
    .profile-1reTe{
        margin-top:.4rem;
        background:#F5F5F5;
        .myorder{
            padding-left:1.6rem;
            display:flex;
            align-items: center;
            aside{
                @include wh(.7rem,.7rem);
                margin-left:-.866667rem;
                margin-right:.266667rem;
                display:flex;
                align-items: center;
                svg{
                    @include wh(100%,100%);
                }
            }
            .myorder-div{
                width:100%;
                border-bottom:1px solid #f1f1f1;
                padding:.433333rem .266667rem .433333rem 0;
                @include sc(.7rem,#333);
                display:flex;
                justify-content:space-between;
                span{
                    display:block;
                }
                .myorder-divsvg{
                    @include wh(.46667rem,.466667rem);
                    svg{
                        @include wh(100%,100%);
                    }
                }
            }
        }
        .myorder:nth-of-type(3) .myorder-div{
            border:0;
        }
    }
    .content-box{
        display:flex;
            .b-right{
                border-right:.05rem solid $blue;
            }
            .content_num-div{
                text-align: center;
                flex:auto;
                // height: 4rem;
                // line-height: 4rem;
                
                span{
                    font-size: .5rem !important;
                    color:#333 !important;
                    // @include sc(.5rem, #fff);
                }
                span:nth-of-type(2){
                    font-size: .7rem !important;
                    color:$blue !important;

                }
            }
    }
</style>

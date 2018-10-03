<template>
    <div>
        <div class="loading-section" v-if="reloadFlag">
            <img src="../assets/images/loading.gif" alt="">
            <span>松手刷新</span>
        </div>
        <div class="home" @touchstart="touchStart" @touchmove="touchMove" @touchend="touchEnd" ref="home">
            <div class="list" v-for="(item, index) in list" :key="index" >第{{index}}个list</div>
        </div>
    </div>
</template>

<script>
    export default {
        data(){
            return {
                list:[{}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}],
                startY: 0,
                moveY: 0,
                disY: 0,
                tempDisY: 0,
                reloadFlag: false,
            }
        },
        methods: {
            touchStart(e){
                e = e || event;
                //tounches类数组，等于1时表示此时有只有一只手指在触摸屏幕
                if(e.touches.length == 1){
                    this.startY = e.touches[0].clientY; //记录开始位置
                }
            },

            touchMove(e){
                e = e || event;
                // e.preventDafault();
                if(e.touches.length == 1){
                    //滑动时距离浏览器左侧的距离
                    this.moveY = e.touches[0].clientY;

                    //实时的滑动的距离-起始位置=实时移动的位置
                    this.disY = this.moveY - this.startY;
                    if(Number(window.scrollY) == 0){
                        this.tempDisY = this.tempDisY == 0 ? this.disY : this.tempDisY;
                        this.removeTransition(); //清除过渡
                        let translateY = Math.pow(this.disY - this.tempDisY, 0.9);
                        if(translateY > 100){
                            this.reloadFlag = true;
                        }
                        this.setTranslateY(translateY);
                    }
                }
            },

            touchEnd(e){
                e = e || event;
                // e.preventDafault();
                if(e.changedTouches.length == 1){
                    this.addTransition();    //加过渡动画
                    let $dom = this.$refs.home;
                    this.setTranslateY(0);
                    this.reloadFun();
                }
            },

            //公用方法
            //加过渡
            addTransition(){
                let $dom = this.$refs.home;
                $dom.style.transition = "all 0.3s";
                $dom.style.webkitTransition = "all 0.3s";/*做兼容*/
            },
            //清除过渡
            removeTransition(){
                let $dom = this.$refs.home;
                $dom.style.transition = "none";
                $dom.style.webkitTransition = "none";
            },
            //定位
            setTranslateY(translateY){
                let $dom = this.$refs.home;
                $dom.style.transform = "translateY("+translateY+"px)";
                $dom.style.webkitTransform = "translateY("+translateY+"px)";
            },

            //刷新函数
            reloadFun(){
                this.reloadFlag = false;
                
            }
        },
        mounted(){
            
        }
    };
</script>

<style lang="scss">
    .home{
      background-color: #E9EAF1;
      padding-top: 10px;
      padding-bottom: 10px;
        .list{
            width: 80%;
            height: 50px;
            line-height: 50px;
            margin-left: 10%;
            margin-bottom: 10px;
            margin-top: 10px;
            background-color: rgb(211, 209, 209);
            border-radius: 8px;
        }
    }

    .loading-section{
        width: 100%;
        height: 80px;
        text-align: center;
        line-height: 50px;

        img{
            width: 16px;
            height: 16px;
            // margin-top: 25px;
        }

        span{
            font-size: 16px;
        }
    }
</style>
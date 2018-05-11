<template>

<div id="app">
    <div id="header"> <img src="./assets/left.png" alt="" class="img_left" :class="{show_left:show}" @click="back"> {{header}}  </div>
    <transition :name="transitionName">
        <router-view class="Router"></router-view>
    </transition>
</div>
</template>

<script>
export default {
    name: 'App',
    data() {
        return {
            transitionName: 'slide-right', // 默认动态路由变化为slide-right
            header:'Index',
            show:false
        }
    },
    　　watch: {　　　
        '$route' (to, from) {　
            this.header = to.name;
            this.header=='Index'?this.show=false:this.show=true;
            console.log(this.show)
            if (to.path == '/') {
                this.transitionName = 'slide-right';
            } else {
                this.transitionName = 'slide-left';
            }
        }　
    },
    methods:{
        back(){
            this.$router.back(-1)
        }
    }

}
</script>

<style>
*{
  margin: 0;
  padding: 0;
}
#app {
    font-family: "Helvetica Neue", Helvetica, "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", "微软雅黑", Arial, sans-serif;
}

.Router {
    position: absolute;
     width: 95%;
     transition: all .8s ease;
     top: 50px;
     left:2.5%
}

.slide-left-enter,
.slide-right-leave-active {
    opacity: 0;
    -webkit-transform: translate(100%, 0);
    transform: translate(100%, 0);
}

.slide-left-leave-active,
.slide-right-enter {
    opacity: 0;
    -webkit-transform: translate(-100%, 0);
    transform: translate(-100% 0);
}
#header{
    height: 44px;
    text-align: center;
    line-height: 44px;
    background: #409EFF;
    color: #fff
}
.img_left{
    float: left;
    width: 30px;
    height: 30px;
    position: absolute;
    top: 7px;
    left: 10px;
    display: none
}
.show_left{
    display: block
}
</style>

<template>
<div id="app">
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
            transitionName: 'slide-right' // 默认动态路由变化为slide-right
        }
    },
    　　watch: {　　　
        '$route' (to, from) {　　　　
            if (to.path == '/') {
                this.transitionName = 'slide-right';
            } else {
                this.transitionName = 'slide-left';
            }
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
     top: 20px;
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
</style>

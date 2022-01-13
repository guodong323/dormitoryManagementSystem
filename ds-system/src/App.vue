<template>
<div id="App">
    <router-view />
</div>
</template>

<script>
export default {
    name: 'App',
    data() {
        return {

        }
    },
    components: {},
    //生命周期 - 创建完成（访问当前this实例）
    created() {
        //在页面刷新时将vuex里的信息保存到localStorage里
        window.addEventListener("beforeunload",()=>{
        localStorage.setItem("messageStore",JSON.stringify(this.$store.state))
    })
    //在页面加载时读取localStorage里的状态信息
    localStorage.getItem("messageStore") && this.$store.replaceState(Object.assign(this.$store.state,JSON.parse(localStorage.getItem("messageStore"))));
    },
    //生命周期 - 挂载完成（访问DOM元素）
    mounted() {

    }
}
</script>



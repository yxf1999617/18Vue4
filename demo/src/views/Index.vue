<template>
    <div class="index">
        <router-link to="/login">登录</router-link>
        <router-link to="/register">注册</router-link>
    </div>
</template>

<script>
    export default {
        name: "Index",
        mounted() {
            if(this.$cookies.isKey('username')){
                let loginuser=this.$cookies.get('username')
                this.$store.commit('setLoginUser',loginuser)
                this.$router.push('/productList');
            }
        }
    }
</script>

<style scoped>

</style>
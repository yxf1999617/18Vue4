<template>
    <div class="login">
        <h3>用户登录</h3>
        <p><input prefix="ios-contact" v-model="username" placeholder="请输入用户名" class="input"></p>
        <p><input prefix="ios-lock" v-model="password" type="password" placeholder="请输入密码" class="input"></p>
        <p>
            <Checkbox v-model="save">登录名和密码保留一周</Checkbox>
        </p>
        <p>
            <button prefix="primary" class="btn" @click="doLogin()">登录</button>
            <button prefix="primary" class="btn" to="/register">注册</button>
        </p>
    </div>
</template>

<script>
    import axios from 'axios'
    export default {
        name: "Login",
        data(){
            return{
                username:'',
                password:'',
                save:''
            }
        },
        methods:{
            doLogin(){
                axios.post('http://localhost:8080/api/login',{username:this.username,password:this.password}).then((res)=>{
                    if(res.data){
                        if(this.save){
                            this.$cookies.set('username',this.username,'7d');
                        }
                        this.$store.commit('setLoginUser',this.username);
                        this.$router.push('/productList');
                    }
                    else{
                        alert('用户名或密码错误，请重新输入');
                        this.username='';
                        this.password='';
                    }
                })
            }
        }
    }
</script>

<style scoped>

</style>
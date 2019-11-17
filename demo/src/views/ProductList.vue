<template>
    <div class="productList">
        <nav>
            <span style="margin-right: 20px">欢迎您：{{this.$store.state.loginuser}}</span>
            <router-link to="/login" @click.native="logout()">登出</router-link>
        </nav>
        <div class="list">
            <product v-for="(item,index) in showlist" :info="item" :key="index"></product>
        </div>
        <div class="page">
            <Page :total="total" :current="current" :page-size="size" prev-text="上一页" next-text="下一页" @on-change="change">
            </Page>
        </div>
    </div>
</template>

<script>
    import Product from '@/components/Product.vue'
    import axios from 'axios'
    export default {
        name: "ProductList",
        data(){
            return{
                prodlist:[],
                current:1,
                size:4,
                showlist:[]
            }
        },
        computed:{
            total(){
                return this.prodlist.length;
            }
        },
        components:{product:Product},
        mounted() {
            if(this.$store.state.loginuser==''){
                this.$router.push('/')
            }else{
                axios.post("http://localhost:8080/api/getproductlist").then((res)=>{
                    this.prodlist=res.data;
                    this.change(this.current);
                });

            }
        },
        methods:{
            logout(){
                if(this.$cookies.isKey('username')){
                    this.$cookies.remove('username')
                }
                this.$store.commit('serLoginUser','')
            },
            change(val){
                this.current=val;
                this.showlist=[];
                for(var i=0;i<this.size;i++){
                    var index=(this.current-1)*this.size+i;
                    if(index<this.prodlist.length){
                        var prod=this.prodlist[index];
                        console.log(prod)
                        this.showlist.push(prod);
                    }
                    else{ break;}

                }
            }
        }
    }
</script>

<style scoped>

</style>
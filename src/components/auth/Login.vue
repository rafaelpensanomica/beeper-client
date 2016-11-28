<template>
    <div>

       <h3>Login</h3>
       <div class="form-group">
           <input type="text" class="form-control" placeholder="Username" v-model="user.username">
       </div>
       <div class="form-group">
           <input type="password" class="form-control" placeholder="Password" v-model="user.password">
       </div>
       <hr>
       <button class="btn btn-lg btn-primary btn-block" @click="login">Sign up!</button>
       <p class="text-center">
           Don't have an account? <router-link to="/auth/register">Register</router-link>
       </p>
    </div>

</template>

<script>
     export default {
        name: "login",
        data:function(){
            return {
                user:{
                    username:'',
                    password:''
                }
            };
        },
        methods:{
            login(){
                 this.$http.post('/auth', this.user)
                .then(function(res){
                    this.$auth.setToken(res.body.token, Date.now() + 14400000);
                    this.$router.push('/newsfeed');
                });
            }
        }
    }
</script>

<style>

</style>

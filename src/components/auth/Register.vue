<template>
   <div>

       <h3>Register</h3>
       <div class="form-group">
          <input type="email" class="form-control" placeholder="Email address" v-model="user.email">
       </div>
       <div class="form-group">
          <input type="text" class="form-control" placeholder="Username" v-model="user.username">
       </div>
       <div class="form-group">   
         <input type="password" class="form-control" placeholder="Password" v-model="user.password">
       </div>
       <hr>
       <button class="btn btn-lg btn-primary btn-block" @click="register">Register</button>
       <p class="text-center">
           Already have an account? <router-link to="/auth/login">Sign up!</router-link>
       </p>
    </div>
</template>

<script>
    export default {
        name: "register",
        data:function(){
            return {
                user: {
                    email:'',
                    username:'',
                    password:''
                }
            };
        },
        methods: {
            register(){
                this.$http.post('/users', this.user)
                .then(function(res){
                    alertify.success("Success! you can now login with your email and password");
                    this.$router.push('/auth/login');
                });
            }
        }
    }
</script>

<style>

</style>

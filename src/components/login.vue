<template>
  <div class="container-fluid" style="height:100%" id="test">
     <img src="../logo/white_notify.png" alt="" class="mx-auto d-block pt-5">
    <div class="row pt-5 d-flex justify-content-center" >
      <div class="col-sm-4"></div>
      <div class="col-sm-3 mt-5 pr-0 pl-0 pb-0" id="form">
        <form @submit.prevent="login" class="pt-5" >
          <h3 style="color:#164e87" class="text-center mb-5">Log in to your account</h3>
        <div class="form-group row d-flex justify-content-center pt-5" >
        <v-icon class="mdi mdi-email mr-1" large color="#164e87"></v-icon>
        <input 
          type="email"
          class="form-control"
          v-model="loginInfo.email"
          aria-describedby="emailHelp"
          required
          style="width:50%;"
          placeholder="Email address"
        /> 
      </div>
      <div class="form-group row d-flex justify-content-center">
      <v-icon class="mdi mdi-lock mr-1" large color="#164e87"></v-icon>
      <input type="password" v-model="loginInfo.password" class="form-control " style="width:50%" id="p1" required placeholder="Password" />
      </div>
      <div class="d-flex justify-content-center">
    <v-btn type="submit" class="white--text " style="width:30%;" color="#164e87">Log In</v-btn>
      </div>
       </form>
      <p class="d-flex justify-content-center mt-5 mb-0" id="info1">
            <span class="mt-2">New to Notify? </span> <router-link class="ml-1 mt-2" to="register" style="color:#164e87">Sign Up</router-link>
      </p>
      <a id="info"
        style="color:#164e87"
        class="d-flex justify-content-center pb-3 pt-1"
        @click="forgotpassword"
        href="#"
      >Forgot your password?</a>
      </div>
      <div class="col-sm-4"></div>
    </div>
    <div class="fixed-bottom white--text text-center mb-5">@ 2020 Notify.com</div>
  </div>
</template>

<script>
import api from "../api";
export default {
  data() {
    return {
      loginInfo: {
        email: null,
        password: null
      }
    };
  },
  methods: {
    login() {
      if (this.loginInfo.password.length < 5)
        return this.$toasted.error("Password must be atleast 5 characters");
      if (/[A-Z]/.test(this.loginInfo.password) == false)
        return this.$toasted.error(
          "Password must contain atleast one uppercase letter"
        );
      this.$loading(true);
      api
        .login(this.loginInfo)
        .then(response => {
          this.$loading(false);
          this.$store.commit("assignUser", response.data);
          this.$router.push("/home");
        })
        .catch(err => {
          this.$loading(false);
          this.$toasted.error(err.response.data);
        });
    },
    forgotpassword() {
      this.$prompt(
        "A password reset email will be sent",
        null,
        "Enter your email",
        "question",
        {input:'email',
        allowOutsideClick:false}
      )
        .then(text => {
          this.$loading(true);
         api.changepassword(text).then(()=>{
           this.$loading(false);
           this.$alert('Check your email for password reset link','An email has been sent','success');
         }).catch((err)=>{this.$loading(false);this.$alert(err.response.data,'Error','error');});
        });
    }
  },

  created() {
    api.deletetoken();
    
  }
};
</script>

<style scoped>
#form{
  border: 1px solid #164e87;
  border-radius: 50px;
  background: #fff;
}
#test{
  background:#164e87
}
#info1{
    background: rgba(211, 211, 211, 0.897);
}
#info{
  border-bottom: 1px solid black;
  border-bottom-left-radius: 50px;
   border-bottom-right-radius: 50px;
  background: rgba(211, 211, 211, 0.897)
  
}

</style>
<template>
  <div class="login">
    <div :style="{display: loginShow ? 'block' : 'none'}">
      <h2 class="login-header">Log in</h2>x
      <form method="POST" class="login-container" @submit.prevent="login">
        <p>
          <input type="email" placeholder="Email" v-model="email" />
        </p>
        <p>
          <input type="password" placeholder="Password" v-model="password" />
        </p>
        <p>
          <input type="submit" value="Log in" />
        </p>
        <p>
          don't have account?
          <span @click="turnRegister">register here</span>
        </p>
      </form>
    </div>
    <div :style="{display: registerShow ? 'block' : 'none'}">
      <h2 class="login-header">Register</h2>x
      <form method="POST" class="login-container" @submit.prevent="register">
        <p>
          <input type="email" placeholder="Email" v-model="email" />
        </p>
        <p>
          <input type="name" placeholder="Name" v-model="name" />
        </p>
        <p>
          <input type="password" placeholder="Password" v-model="password" />
        </p>
        <p>
          <input type="submit" value="Register" />
        </p>
        <p>
          already have account?
          <span @click="turnRegister">login here</span>
        </p>
      </form>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "Login",
  data() {
    return {
      registerShow: false,
      loginShow: true,
      email: null,
      password: null,
      name: null
    };
  },
  methods: {
    turnRegister() {
      this.registerShow = !this.registerShow;
      this.loginShow = !this.loginShow;
    },
    register() {
      axios({
        method: "POST",
        url: "https://sleepy-fjord-45887.herokuapp.com/users/register",
        data: {
          email: this.email,
          password: this.password,
          name: this.name
        }
      })
        .then(data => {
          localStorage.setItem("token", data.data.token);
          localStorage.setItem("role", data.role);
          console.log(data);
          if (data.data.role === "user") {
            this.$router.push({ name: "Home" });
          } else {
            this.$router.push({ name: "Content" });
          }
        })
        .catch(err => {
          Swal.fire(err.response.data);
        });
    },
    login() {
      axios({
        method: "POST",
        url: `https://sleepy-fjord-45887.herokuapp.com/users/login`,
        data: {
          email: this.email,
          password: this.password
        }
      })
        .then(data => {
          localStorage.setItem("token", data.data.token);
          localStorage.setItem("role", data.data.role);
          if (data.data.role === "user") {
            this.$emit("isLogin");
            this.$router.push({ name: "Home" });
          } else {
            this.$router.push({ name: "Content" });
          }
        })
        .catch(err => {
          this.$Swal.fire(err.response.data);
        });
    }
  }
};
</script>
<style >
</style>

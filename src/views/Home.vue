<template>
  <div class="login">
    <div class="head">
      <div>
        <img
          class="responsive"
          src="../assets/logo.jpg"
          width="100"
          height="100"
        />
      </div>
      <div>
        <img
          class="responsive"
          src="../assets/sharp.jpg"
          width="200"
          height="100"
        />
      </div>
    </div>
    <h1>{{ heading }}</h1>
    <div class="earth">
      <div class="in">
        <input
          class="loginput"
          type="email"
          name="email"
          placeholder="email"
          v-on:keyup.enter="login()"
          v-model="emInput"
        />
        <br />
        <input
          class="loginput"
          type="password"
          name="password"
          v-on:keyup.enter="login()"
          placeholder="password"
          v-model="passInput"
        />
        <br />
        <button class="submitbg" v-on:click="login()">Login</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      heading: "GK QUIZ(MAKE YOUR MIND SHARPER)",
      email: "syntist@it.com",
      pass: "12345",
      emInput: "",
      passInput: ""
    };
  },
  methods: {
    login: function() {
      if (this.email == this.emInput && this.pass == this.passInput) {
        this.$router.push("game");
      } else {
        this.$alert("Wrong, Email or Passowrd");
      }
    }
  }
};
</script>

<style scoped>
.login {
  margin: auto;
  text-align: center;
  padding-bottom: 5%;
}

.login .head {
  padding: 5px 9px 5px 5px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.login .earth {
  background-image: url("../assets/earth.jpeg");
  background-size: auto;
  background-repeat: no-repeat;
  background-position: center;
  border-radius: 100px;
  margin: auto;
  height: 500px;
  width: 100%;
  max-width: 500px;
}

.login .in {
  margin: auto;
  padding-top: 180px;
}

.login .loginput {
  text-align: center;
  border-radius: 20px;
  padding: 10px;
  margin-bottom: 30px;
}

.login .submitbg {
  text-align: center;
  padding: 10px;
}

.responsive {
  max-width: 100%;
  height: auto;
}

body {
  font-size: calc(0.5em + 1vmin);
}
</style>

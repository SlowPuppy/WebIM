<template>
  <div class="container" :class="{ 'sign-up-mode': isSignup }">
    <form class="form" :class="{ 'sign-up-mode': isSignup }">
      <transition name="fade">
        <h2 class="title" v-show="isSignup">注册</h2>
      </transition>
      <transition name="fade">
        <h2 class="title" v-show="!isSignup">登录</h2>
      </transition>
      <div class="input-field">
        <i class="fa fa-user"></i>
        <input type="text" placeholder="用户名" v-model="username" value="2357561424"/>
      </div>
      <div class="input-field">
        <i class="fa fa-lock"></i>
        <input type="password" placeholder="密码" v-model="password" value="123456"/>
      </div>
      <transition name="fade">
        <div class="input-field" v-show="isSignup">
          <i class="fa fa-id-card-o"></i>
          <input type="text" placeholder="昵称" v-model="nickname" />
        </div>
      </transition>
      <transition name="fade">
        <div class="input-field contentplaceholder" v-show="!isSignup">
          <i class="fa contentplaceholder"></i>
          <input class="contentplaceholder" type="text" disabled />
        </div>
      </transition>
      <transition name="fade">
        <button class="btn" v-if="isSignup" @click.prevent="Signup">
          注册
        </button>
      </transition>
      <transition name="fade">
        <button class="btn" v-if="!isSignup" @click.prevent="Login">
          登录
        </button>
      </transition>
      <div>
        <transition name="fade">
          <p class="social-text" v-show="isSignup">或通过社交账号注册</p>
        </transition>
        <transition name="fade">
          <p class="social-text" v-show="!isSignup">或通过社交账号登录</p>
        </transition>
        <div class="social-media">
          <a href="#" class="social-icon">
            <i class="fa fa-qq"></i>
          </a>
          <a href="#" class="social-icon">
            <i class="fa fa-wechat"></i>
          </a>
          <a href="#" class="social-icon">
            <i class="fa fa-weibo"></i>
          </a>
          <a href="#" class="social-icon">
            <i class="fa fa-google"></i>
          </a>
        </div>
      </div>
    </form>
    <transition name="lcontent">
      <Panel class="panel left-panel" v-show="!isSignup">
        <template #title> 没有账号? </template>
        <template #content>
          <p>
            可以登录测试账号使用功能：<br>
            账号1：2357561424<br>
            密码：123456<br>
            账号2：877390550<br>
            密码：123456<br>
          </p>
        </template>
        <template #button>
          <button class="btn transparent" @click="ToSignup">去注册</button>
        </template>
      </Panel>
    </transition>
    <transition name="limage">
      <img
        src="../../assets/log.svg"
        class="left-image image"
        alt=""
        v-show="!isSignup"
      />
    </transition>
    <transition name="rcontent">
      <Panel class="panel right-panel" v-show="isSignup">
        <template #title> 已有账号? </template>
        <template #content>
          <p>
            Lorem ipsum, dolor sit amet consectetur adipisicing elit. Beatae sed
            dicta dolor saepe iusto earum enim, ipsum itaque id ratione? Ex quas
            illo dicta debitis distinctio tempora facilis voluptatem neque.
          </p>
        </template>
        <template #button>
          <button class="btn transparent" @click="ToSignin">去登录</button>
        </template>
      </Panel>
    </transition>
    <transition name="rimage">
      <img
        src="../../assets/register.svg"
        class="right-image image"
        alt=""
        v-show="isSignup"
      />
    </transition>
  </div>
</template>

<script>
import Panel from "../../components/login/Panel.vue";
import { mapMutations, mapActions } from "vuex";
import axios from 'axios'
const userInfo =
  (localStorage.getItem("userInfo") &&
    JSON.parse(localStorage.getItem("userInfo"))) ||
  "";
export default {
  components: { Panel },
  data() {
    return {
      username: userInfo.userId || "",
      password: userInfo.password || "",
      nickname: "",
    };
  },
  computed: {
    isSignup() {
      return this.$store.state.login.isSignup;
    },
  },
  mounted() {
    const path = this.isSignup ? "/signup" : "/login";
    if (path !== location.pathname) {
      this.$router.push(path);
    }
  },

  methods: {
    ...mapMutations(["SETSIGNUPFLAG"]),
    ...mapActions(["onLogin", "onRegister","updateOwnUserInfo"]),

    ToSignin() {
      this.SETSIGNUPFLAG(false);
    },
    ToSignup() {
      this.SETSIGNUPFLAG(true);
    },
    Signup() {
      this.onRegister({
        username: this.username.toLowerCase(),
        password: this.password,
        nickname: this.nickname,
      });
    },
    Login() {
      this.onLogin({
        username: this.username.toLowerCase(),
        password: this.password,
      });
    },
  },
};
</script>

<style>
@import "./index.css";
</style>



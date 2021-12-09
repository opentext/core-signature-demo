<template>
  <div>
    <SystemInfo :showSubscription="false"></SystemInfo>
    <br />
    <button @click="loginToDevx">Login to Devx</button>
    <p v-if="loginStatus && loginStatus === 'success'" class="success">
      <strong
        >&#10003; Logged in successfully as {{ systemInfo.username }}!</strong
      >
    </p>
    <p v-if="loginStatus && loginStatus !== 'success'" class="error">
      <strong>Login failed: {{ loginStatus }}</strong>
    </p>
  </div>
</template>

<script>
import { mapState, mapGetters } from "vuex";
import SystemInfo from "./SystemInfo";

export default {
  name: "DevxLogin",
  components: {
    SystemInfo
  },
  computed: {
    ...mapGetters("auth", ["authBody"]),
    ...mapState("auth", ["loginStatus"]),
    ...mapGetters("systemInfo", ["systemInfo"]),
    authTokenUrl() {
      return `${this.systemInfo.authHost}/tenants/${this.systemInfo.tenant}/oauth2/token`;
    }
  },
  methods: {
    loginToDevx() {
      this.$http
        .post(this.authTokenUrl, this.authBody, { emulateJSON: true })
        .then(result => {
          this.$store.commit("auth/loginSuccess", {
            result: result
          });
        })
        .catch(error => {
          this.$store.commit("auth/loginFailure", {
            error: error
          });
        });
    }
  }
};
</script>
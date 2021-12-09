<template>
  <div class="container">
    <tabs>
      <tab name="Login with OT2" method="ot2" :selected="true">
        <OT2Login />
      </tab>
      <tab name="Login with DevX" method="devx" :selected="false">
        <DevxLogin />
      </tab>
    </tabs>
  </div>
</template>

<script>
import tab from "./tab";
import tabs from "./tabs";
import OT2Login from "./OT2Login";
import DevxLogin from "./DevxLogin";

export default {
  name: "LoginTabs",
  components: {
    DevxLogin,
    OT2Login,
    tab,
    tabs
  }
};
</script>

<style scoped>
.container {
  position: relative;
}
</style>

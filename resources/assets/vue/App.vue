<script lang="ts">
import axios from 'axios';
import { Action, State, namespace } from 'vuex-class'
import { Component, Vue } from 'vue-property-decorator';
import AppHeader from "./components/AppHeader.vue";

const RootState = namespace('Root', State);
const RootAction = namespace('Root', Action);

@Component({
  components: {
    AppHeader,
  },
})
export default class App extends Vue {
  @RootState('loaded') loaded;
  @RootState('appName') appName;

  @RootAction('setData') setData;

  async mounted() {
    this.$router.beforeEach((to, from, next) => {
      document.title = `${to.meta.title} - ${this.appName}`;
      next();
    });

    await this.setData();

    this.$router.onReady(() => {
      document.title = `${this.$router.currentRoute.meta.title} - ${this.appName}`;
    });
  }
}
</script>

<template lang="pug">
div
  dialogs-wrapper/
  app-header/
  router-view(v-if='loaded')/
</template>

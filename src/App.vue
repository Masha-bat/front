<template>
  <div id="app">
    <site-header />
    <site-menu />
    <router-view />
    <site-footer />
  </div>
</template>

<script>
import SiteHeader from './components/siteHeader.vue';
import SiteFooter from './components/siteFooter.vue';
import SiteMenu from './components/siteMenu.vue';

export default {
  components: {
    SiteHeader,
    SiteFooter,
    SiteMenu,
  },
};
</script>

<style>
/* Общие стили */
#app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

/* Стили для контейнера с контентом */
router-view {
  flex: 1;
  padding: 20px;
}
</style>

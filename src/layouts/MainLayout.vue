<template>
  <q-layout view="lHh Lpr lFf">
    <q-header bordered class="bg-white text-grey-10">
      <q-toolbar class="constrain">
        <q-btn
          to="/camera"
          class="large-screen-only"
          flat
          round
          icon="eva-camera-outline"
          size="18px"
          dense
        />
        <q-separator vertical spaced class="large-screen-only" />
        <q-toolbar-title class="text-grand-hotel text-bold">
          Picture-gram
        </q-toolbar-title>
        <q-btn
          to="/"
          class="large-screen-only q-mr-sm"
          flat
          round
          icon="eva-home-outline"
          size="18px"
          dense
        />
      </q-toolbar>
    </q-header>

    <q-footer bordered class="bg-white small-screen-only">
      <q-tabs
        class="text-grey-10"
        active-color="primary"
        indicator-color="transparent"
      >
        <q-route-tab to="/" icon="home" />
        <q-route-tab to="/camera" icon="camera" />
      </q-tabs>
    </q-footer>

    <q-page-container class="bg-grey-1">
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref } from "vue";

export default defineComponent({
  name: "MainLayout",

  setup() {
    return {};
  },
});
</script>

<style lang="scss">
.q-toolbar {
  @media (min-width: 599px) {
    height: 65px;
  }
}
.q-toolbar__title {
  font-size: 30px;
  @media (max-width: 599px) {
    text-align: center;
  }
}
.q-footer {
  .q-tab__icon {
    font-size: 30px;
  }
}
</style>
>

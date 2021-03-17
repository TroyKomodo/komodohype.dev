<template>
  <div class="default">
    <div class="mount-container" :class="{ loaded }">
      <Nuxt class="mount" />
      <div class="background-wrapper"></div>
    </div>
    <Background
      :background-from="[10, 25, 100]"
      :background-to="[25, 50, 150]"
      :background-duration="4000"
      :speed="20"
      :nobg="false"
      :line-color="[255, 255, 255]"
      :line-size="2"
      :particle-size="2"
    />
  </div>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  data() {
    return {
      loaded: false,
    };
  },
  head() {
    return {
      title: "KomodoHype | Portfolio",
      meta: [
        // hid is used as unique identifier. Do not use `vmid` for it as it will not work
        {
          hid: "apple-mobile-web-app-title",
          name: "apple-mobile-web-app-title",
          content: "KomodoHype Portfolio",
        },
        {
          hid: "og:title",
          name: "og:title",
          content: "KomodoHype Portfolio",
        },
        {
          hid: "twitter:title",
          name: "twitter:title",
          content: "KomodoHype Portfolio",
        },
        {
          hid: "og:site_name",
          name: "og:site_name",
          content: "KomodoHype Portfolio",
        },
        {
          hid: "og:url",
          name: "og:url",
          content: `https://komodohype.dev`,
        },
        {
          hid: "twitter:description",
          name: "twitter:description",
          content: "A portfolio page for Troy Benson @ KomodoHype.",
        },
        {
          hid: "og:description",
          name: "og:description",
          content: "A portfolio page for Troy Benson @ KomodoHype.",
        },
        {
          hid: "twitter:image",
          name: "twitter:image",
          content: "/komodohype.webp",
        },
        {
          hid: "og:image",
          name: "og:image",
          content: "/komodohype.webp",
        },
        {
          hid: "twitter:card",
          name: "twitter:card",
          content: "summary_large_image",
        },
        {
          hid: "twitter:site",
          name: "twitter:site",
          content: "@troydota",
        },
        {
          hid: "twitter:image:alt",
          name: "twitter:image:alt",
          content: "KomodoHype",
        },
        {
          hid: "og:type",
          name: "og:type",
          content: "website",
        },
      ],
    };
  },
  mounted() {
    this.loaded = true;
  },
});
</script>

<style lang="scss" scoped>
@import "@/assets/scss/default.scss";
</style>

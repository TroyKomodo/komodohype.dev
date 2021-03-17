<template>
  <div class="index">
    <div class="wrapper">
      <header>
        <h1>Hi, I am Troy</h1>
        <p>
          I am {{ age }} years old and have been programming for about
          {{ year - 2010 }} years.
        </p>
        <p>
          I mainly work as a fullstack software engineer on a commission basis
          for a few Twitch streamers.
        </p>
      </header>
      <main>
        <h3>Frameworks, languages and services I have used</h3>
        <ul>
          <li>AWS - S3, Lambda, Route53 and EC2</li>
          <li>CloudFlare</li>
          <li>Docker</li>
          <li>MongoDB, MySQL, PostgresSQL and Redis</li>
          <li>GraphQL, WebSockets and WebRTC</li>
          <li>React, Next, Nuxt and Vue</li>
          <li>GoLang, Python, JavaScript, TypeScript and Lua</li>
          <li>NodeJS, Deno (a bit)</li>
          <li>Express and GoFiber</li>
          <li>Ffmpeg, Varnish Cache</li>
        </ul>
        <h3>My work experience and projects</h3>
        <ul>
          <li>
            <a href="https://streamervods.live" target="_BLANK">StreamerVods</a>
            - A vod recording site for streamers.
          </li>
          <li>
            <a href="https://github.com/pajbot/pajbot" target="_BLANK"
              >pajbot</a
            >
            - An opensource Twitch bot for fun and moderation.
          </li>
          <li>
            <a href="https://viders.tv" target="_BLANK">VidersTV</a> - A private
            streaming platform.
          </li>
          <li>
            <a href="https://github.com/troydota/modlogs" target="_BLANK"
              >ModLogs</a
            >
            - An open source moderation logging tool for Twitch.
          </li>
        </ul>
        <h3>Twitch streamers I have worked with</h3>
        <ul>
          <li>
            <a href="https://twitch.tv/admiralbulldog" target="_BLANK"
              >AdmiralBulldog</a
            >
          </li>
          <li><a href="https://twitch.tv/lacari" target="_BLANK">Lacari</a></li>
          <li>
            <a href="https://twitch.tv/quinndota" target="_BLANK">Quinn</a>
          </li>
          <li>
            <a href="https://twitch.tv/gunnardota2" target="_BLANK">Gunnar</a>
          </li>
          <li>
            <a href="https://twitch.tv/monkeys_forever" target="_BLANK"
              >Monkeys Forever</a
            >
          </li>
          <li>
            <a href="https://twitch.tv/lukiluki" target="_BLANK">LukiLuki</a>
          </li>
          <li><a href="https://twitch.tv/bububu" target="_BLANK">Bububu</a></li>
          <li>
            <a href="https://twitch.tv/invokergirl" target="_BLANK"
              >InvokerGirl</a
            >
          </li>
        </ul>
      </main>
      <footer>
        <div class="logo" @mouseenter="xqcL = true" @mouseleave="xqcL = false">
          <img class="xqcL" :class="{ shown: xqcL }" src="@/static/xqcL.webp" />
          <img
            class="komodohype"
            :class="{ hidden: xqcL }"
            src="@/static/komodohype.webp"
          />
        </div>
        <div class="contact">
          © KomodoHype 2021. Email:
          <a href="mailto:troy@komodohype.dev" class="email-link"
            >troy@komodohype.dev</a
          >
          <span
            >The Logos were made by
            <a href="https://twitch.tv/tendieconnoisseur" target="_BLANK"
              >TendieConnoisseur</a
            ></span
          >
        </div>
        <div class="socials">
          <a
            href="https://github.com/troydota"
            target="_BLANK"
            class="github silent"
            ><font-awesome-icon :icon="['fab', 'github']"
          /></a>
          <a
            href="https://twitter.com/troydota"
            target="_BLANK"
            class="twitter silent"
            ><font-awesome-icon :icon="['fab', 'twitter']"
          /></a>
          <a
            href="https://twitch.tv/troydota"
            target="_BLANK"
            class="twitch silent"
            ><img src="@/static/twitch.svg"
          /></a>
          <a
            href="https://discordapp.com/users/383195095610163200"
            target="_BLANK"
            class="discord silent"
            ><img src="@/static/discord.webp"
          /></a>
          <a
            href="https://linkedin.com/in/troy-benson"
            target="_BLANK"
            class="linkedin silent"
            ><img src="@/static/linkedin.webp"
          /></a>
        </div>
      </footer>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  data() {
    const today = new Date();
    let age = today.getUTCFullYear() - 2002;

    if (today.getUTCMonth() < 3) age--;
    if (today.getUTCMonth() === 3 && today.getDate() < 9) age--;

    return {
      age,
      year: today.getUTCFullYear(),
      xqcL: false,
    };
  },
});
</script>

<style lang="scss" scoped>
@import "@/assets/scss/index.scss";
</style>

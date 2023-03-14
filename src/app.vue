<template lang="pug">
#app
  tm-cookie-consent
  tm-modal-error(
    v-if="nodes.length === 0"
    title="Heimdall Explorer is offline."
    body="The current testnet is offline. The next testnet will be launching soon. Follow us on Twitter to get notified when the next testnet starts."
    btn-icon="forum" btn-value="GitHub repository" btn-url="https://github.com/maticnetwork/tendermint-explorer")
  app-header
  #app-content
    router-view
    app-footer
</template>

<script>
import requestInterval from "request-interval"
import { mapGetters } from "vuex"
import { TmCookieConsent, TmModalError } from "@tendermint/ui"
import AppFooter from "./components/AppFooter"
import AppHeader from "./components/AppHeader"
import store from "./store/index"
export default {
  name: "app",
  components: {
    AppHeader,
    AppFooter,
    TmCookieConsent,
    TmModalError,
  },
  computed: {
    ...mapGetters(["nodes"]),
  },
  mounted() {
    this.$store.dispatch("getLastBlock")
    this.$store.dispatch("subNewBlock")
    this.$store.dispatch("subRoundStep")
    requestInterval(1000, () => this.$store.dispatch("getConsensusState"))
    this.$store.dispatch("getStatus")
    this.$store.dispatch("getNodes")
    this.$store.dispatch("getValidators")
  },
  store,
}
</script>

<style lang="stylus" src="./styles/app.styl"></style>
<style lang="stylus">
  @import url('https://fonts.googleapis.com/css2?family=Space+Mono&display=swap')
  @font-face {
    font-family: './assets/fonts/Helvetica Neue LT Pro';
    src: url('./assets/fonts/HelveticaNeueLTPro-Cn.woff2') format('woff2'),
        url('./assets/fonts/HelveticaNeueLTPro-Cn.woff') format('woff');
    font-weight: normal;
    font-style: normal;
    font-display: swap;
}

@font-face {
    font-family: './assets/fonts/Helvetica Neue LT Pro';
    src: url('./assets/fonts/HelveticaNeueLTPro-BdCn.woff2') format('woff2'),
        url('./assets/fonts/HelveticaNeueLTPro-BdCn.woff') format('woff');
    font-weight: bold;
    font-style: normal;
    font-display: swap;
}

  .tm-part
    max-width 100%
  .tm-part-main
   padding-right 0
  .tm-part-title
    font-size 14px
  .tm-page-header-text
    h2
      font-weight normal
  .tm-li-dl
    height 100%
    padding 0.5rem
  .tm-li-dt
    color var(--dim)
  .tm-li-dd
    white-space normal
    overflow auto
    word-break break-word
  @media screen and (min-width: 768px)
    .tm-part-container:last-child
      padding-bottom: 0
</style>

<template>
  <v-app id="inspire">
    <v-navigation-drawer
      v-if="shouldShowSidebar"
      v-model="drawer"
      :clipped="$vuetify.breakpoint.lgAndUp"
      app
    >
      <v-treeview
        item-key="path"
        item-text="title"
        :items="sidebarItems"
        dense
        open-all
        hoverable
        open-on-click
        activatable
        @update:active="updateActive"
      >
      </v-treeview>
    </v-navigation-drawer>

    <v-app-bar
      :clipped-left="$vuetify.breakpoint.lgAndUp"
      app
      color="blue darken-3"
      dark
    >
      <v-app-bar-nav-icon
        @click.stop="drawer = !drawer"
        v-if="shouldShowSidebar"
      />
      <v-toolbar-title
        style="width: 300px"
        class="ml-0 pl-4 text--white"
      >
        <routerLink :to="$localePath">
          <img
            v-if="$site.themeConfig.logo"
            :src="$withBase($site.themeConfig.logo)"
            :alt="$siteTitle"
          >
          <span
            v-if="$siteTitle"
            ref="siteName"
            :class="{ 'can-hide': $site.themeConfig.logo }"
            class="white--text"
          >{{ $siteTitle }}</span>
        </routerLink>
      </v-toolbar-title>
      <v-spacer />
      <SearchBox />
      <NavLinks class="d-flex" />
    </v-app-bar>
    <v-content>

      <v-row
        no-gutters
        align="center"
        justify="center"
      >
        <v-col
          cols="12"
          sm="12"
          md="8"
        >
          <Home v-if="$page.frontmatter.home" />

          <Page
            v-else
            :sidebar-items="sidebarItems"
          >
            <template #top>
              <slot name="page-top" />
            </template>
            <template #bottom>
              <slot name="page-bottom" />
            </template>
          </Page>

        </v-col>
      </v-row>
    </v-content>
  </v-app>
</template>

<script>
import Home from '@theme/components/Home.vue'
import SearchBox from '@theme/components/SearchBox.vue'
import NavLinks from '@theme/components/NavLinks.vue'
import Page from '@theme/components/Page.vue'
import { resolveSidebarItems } from '../util'

export default {
  props: {
    source: String,
  },
  components: {
    Home,
    Page,
    NavLinks,
    SearchBox
  },

  data: () => ({
    dialog: false,
    drawer: null
  }),

  computed: {
    shouldShowNavbar () {
      const { themeConfig } = this.$site
      const { frontmatter } = this.$page
      if (
        frontmatter.navbar === false
        || themeConfig.navbar === false) {
        return false
      }
      return (
        this.$title
        || themeConfig.logo
        || themeConfig.repo
        || themeConfig.nav
        || this.$themeLocaleConfig.nav
      )
    },
    shouldShowSidebar () {
      const { frontmatter } = this.$page
      return (
        !frontmatter.home
        && frontmatter.sidebar !== false
        && this.sidebarItems.length
      )
    },
    sidebarItems () {
      const items = resolveSidebarItems(
        this.$page,
        this.$page.regularPath,
        this.$site,
        this.$localePath
      )

      return items
    },
  },

  methods: {
    updateActive (active) {
      if (active[0] == undefined) return
      const path = active[0].replace('.html', '')

      this.$router.push(path)
    }
  }
}
</script>

<style src="prismjs/themes/prism-okaidia.css"></style>
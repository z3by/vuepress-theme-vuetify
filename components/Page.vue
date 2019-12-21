<template>
  <main class="ma-4 page fill-height">
    <v-card class="px-7 py-3">
      <slot name="top" />

      <Content class="theme-default-content" />
      <slot name="bottom" />
    </v-card>
    <PageEdit />
    <PageNav v-bind="{ sidebarItems }" />
  </main>
</template>

<script>
import PageEdit from '@theme/components/PageEdit.vue'
import PageNav from '@theme/components/PageNav.vue'

export default {
  components: { PageEdit, PageNav },
  props: ['sidebarItems']
}
</script>

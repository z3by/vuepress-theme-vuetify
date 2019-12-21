<template>
  <v-autocomplete
    v-model="select"
    :loading="loading"
    :items="items"
    :search-input.sync="search"
    cache-items
    class="mx-4"
    flat
    clearable
    hide-no-data
    hide-details
    item-text="title"
    item-value="path"
    label="Search"
    append-icon="mdi-magnify"
    solo-inverted
  ></v-autocomplete>

</template>

<script>
import Flexsearch from "flexsearch";

export default {
  data () {
    return {
      index: null,
      loading: false,
      items: [],
      search: null,
      select: null,
      results: [{}]
    }
  },

  watch: {
    search (val) {
      val && val.length > 1 && val !== this.select && this.querySelections(val)
    },

    select (val) {
      if (val) {
        this.$router.push(val)
      }
    }
  },

  mounted () {
    this.index = new Flexsearch({
      tokenize: "forward",
      doc: {
        id: "key",
        field: ["title"]
      }
    });
    const { pages } = this.$site;
    this.index.add(pages);
  },

  methods: {
    querySelections (v) {
      this.loading = true

      this.index.search(
        v,
        {
          limit: 10,
          threshold: 2,
          encode: 'extra'
        },
        (result) => {
          this.items = result
          this.loading = false
        })
    },
  },
}
</script>
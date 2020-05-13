<template>
  <v-list v-if="Array.isArray(results) || typeof results === 'object'">
    <v-list-item-title v-text="label" v-if="label"></v-list-item-title>
    <v-list-item-content>
      <list-inner
        v-for="(result, index) in results"
        v-bind:key="index"
        v-bind:results="result"
        v-bind:label="Array.isArray(results) ? '' : index"
      />
    </v-list-item-content>
  </v-list>
  <span class="inner-content" v-else v-text="label + ':' + results"></span>
  <!-- <v-list-item-subtitle v-text="results"></v-list-item-subtitle> -->
</template>

<script>
import ListInner from '~/components/ListInner'

export default {
  data: () => ({}),
  name: 'ListInner',
  props: ['results', 'label'],
  components: { ListInner },
  computed: {}
}
</script>

<style>
.inner-content {
  color: #5c5c5c;
  padding: 0.2em 1em;
}
</style>
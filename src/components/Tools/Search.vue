<template>
  <q-input
    v-model="searchField"
    label="Search"
    outlined
  >
    <template v-slot:append>
      <q-icon
        v-if="searchField !== ''"
        name="close"
        @click="searchField = ''"
        class="cursor-pointer"
      />
      <q-icon name="search"/>
    </template>
  </q-input>
</template>

<script>
  import {mapState} from 'vuex';

  export default {
    computed: {
      ...mapState('tasks', ['search']),

      searchField: {
        get() {
          return this.search;
        },

        set(value) {
          this.$store.dispatch('tasks/setSearch', value);
        }
      }
    },
  };
</script>

<style scoped>
  .q-input {
    flex-grow: 1;
  }
</style>

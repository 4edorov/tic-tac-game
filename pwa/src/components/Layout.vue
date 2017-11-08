<template>
  <q-layout>
    <q-toolbar slot="header" class="glossy">
      <q-toolbar-title>
        Tic Tac Toe Game
      </q-toolbar-title>
    </q-toolbar>

    <router-view />

  </q-layout>
</template>

<script>
export default {
  name: 'index',
  data () {
    return {}
  }
}
</script>

<style lang="stylus">
</style>

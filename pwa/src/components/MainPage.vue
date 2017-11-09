<template>
  <div class="layout-padding">
    <div class='substrate shadow-7'>
      <table class='q-table cell-separator loose board'>
        <tbody>
          <tr>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td></td>
            <td></td>
            <td></td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class='panel'>
      <q-collapsible icon="settings" label="Settings" class='bg-info'>
        <div>
          Content
        </div>
      </q-collapsible>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {}
  }
}
</script>

<style lang='stylus'>
@import '~variables'

.substrate
  width 350px
  height 350px
  margin auto
  background-color $tertiary

.board
  width 300px
  height 300px
  background-color $green-14
  margin-top 25px
  margin-left 25px
  display inline-table

.panel
  width 350px
  margin auto
  margin-top 35px
</style>

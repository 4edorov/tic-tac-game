<template>
  <div class='layout-padding'>
    <div class='up-panel'>
      <q-card inline>
        <q-item>
          <q-item-side avatar='../statics/player_1.png' />
          <q-item-main>
            <q-item-tile label>Player 1</q-item-tile>
          </q-item-main>
          <q-list>
          </q-list>
        </q-item>
      </q-card>
      <q-card inline>
        <q-item>
          <q-item-side avatar='../statics/player_1.png' />
          <q-item-main>
            <q-item-tile label>Player 1</q-item-tile>
          </q-item-main>
          <q-list>
          </q-list>
        </q-item>
      </q-card>
    </div>
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
    <div class='down-panel shadow-8'>
      <q-collapsible icon="settings" label="Settings" class='bg-info'>
        <div>
          <p class='caption'>Number of players</p>
          <q-radio v-model='numberOfPlayers' val='1' label='1 player' />
          <q-radio v-model='numberOfPlayers' val='0' label='2 players' />
          <p class='caption'>Noughts or crosses</p>
          <q-radio v-model='firstPlayer' val='0' label='Os' />
          <q-radio v-model='firstPlayer' val='1' label='Xs' />
        </div>
      </q-collapsible>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      firstPlayer: '0',
      numberOfPlayers: '1'
    }
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

.up-panel
  width 350px
  margin auto
  margin-bottom 35px
  display flex
  justify-content space-between

.down-panel
  width 350px
  margin auto
  margin-top 35px
</style>

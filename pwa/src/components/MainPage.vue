<template>
  <div class='layout-padding'>
    <div class='up-panel'>
      <q-card inline class='shadow-7' :color='firstIsActive ? "green" : "red"'>
        <q-item>
          <q-item-side avatar='../statics/player_1.png' />
          <q-item-main>
            <q-item-tile label>Player 1</q-item-tile>
          </q-item-main>
        </q-item>
        <q-list>
          <q-item>
            <q-item-side>
              <img class='player-figure' :src='getFirstFigure' />
            </q-item-side>
            <q-item-main label>Score: {{ firstScore }}</q-item-main>
          </q-item>
        </q-list>
      </q-card>
      <q-card inline class='shadow-7' :color='firstIsActive ? "red" : "green"'>
        <q-item>
          <q-item-side :avatar='avatarPath' />
          <q-item-main>
            <q-item-tile label>{{ secondPlayerName }}</q-item-tile>
          </q-item-main>
        </q-item>
        <q-list>
          <q-item>
            <q-item-side>
              <img class='player-figure' :src='getSecondFigure' />
            </q-item-side>
            <q-item-main label>Score: {{ secondScore }}</q-item-main>
          </q-item>
        </q-list>
      </q-card>
    </div>
    <div class='substrate shadow-7'>
      <table class='q-table cell-separator loose board'>
        <tbody>
          <tr>
            <td id='0' @click='pushSquare($event)'>
              <img id='0' class='figure' v-if='state[0].isActive' :src='state[0].figureSource' />
            </td>
            <td id='1' @click='pushSquare($event)'>
              <img id='1' class='figure' v-if='state[1].isActive' :src='state[1].figureSource' />
            </td>
            <td id='2' @click='pushSquare($event)'>
              <img id='2' class='figure' v-if='state[2].isActive' :src='state[2].figureSource' />
            </td>
          </tr>
          <tr>
            <td id='3' @click='pushSquare($event)'>
              <img id='3' class='figure' v-if='state[3].isActive' :src='state[3].figureSource' />
            </td>
            <td id='4' @click='pushSquare($event)'>
              <img id='4' class='figure' v-if='state[4].isActive' :src='state[4].figureSource' />
            </td>
            <td id='5' @click='pushSquare($event)'>
              <img id='5' class='figure' v-if='state[5].isActive' :src='state[5].figureSource' />
            </td>
          </tr>
          <tr>
            <td id='6' @click='pushSquare($event)'>
              <img id='6' class='figure' v-if='state[6].isActive' :src='state[6].figureSource' />
            </td>
            <td id='7' @click='pushSquare($event)'>
              <img id='7' class='figure' v-if='state[7].isActive' :src='state[7].figureSource' />
            </td>
            <td id='8' @click='pushSquare($event)'>
              <img id='8' class='figure' v-if='state[8].isActive' :src='state[8].figureSource' />
            </td>
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
import vars from '../libs/libs'

export default {
  data () {
    return {
      firstPlayer: '0',
      numberOfPlayers: '0',
      firstScore: 0,
      secondScore: 0,
      state: vars.initialState,
      firstIsActive: true,
      noughtArray: [],
      crossArray: []
    }
  },
  computed: {
    secondPlayerName: function () {
      return parseInt(this.numberOfPlayers, 10) ? 'Computer' : 'Player 2'
    },
    avatarPath: function () {
      return parseInt(this.numberOfPlayers, 10) ? '../statics/computer.png' : '../statics/player_2.png'
    },
    getFirstFigure: function () {
      return this.firstPlayer === '0' ? '../statics/nought.png' : '../statics/cross.png'
    },
    getSecondFigure: function () {
      return this.firstPlayer === '0' ? '../statics/cross.png' : '../statics/nought.png'
    }
  },
  methods: {
    pushSquare (event) {
      if (!this.state[event.target.id].isActive) {
        this.state[event.target.id].isActive = !this.state[event.target.id].isActive
        if ((this.firstPlayer === '0' && this.firstIsActive) || (this.firstPlayer === '1' && !this.firstIsActive)) {
          this.state[event.target.id].figureSource = '../statics/nought.png'
        }
        if ((this.firstPlayer === '1' && this.firstIsActive) || (this.firstPlayer === '0' && !this.firstIsActive)) {
          this.state[event.target.id].figureSource = '../statics/cross.png'
        }
        this.firstIsActive = !this.firstIsActive

        if (this.state[event.target.id].figureSource === '../statics/nought.png') {
          this.noughtArray.push(parseInt(event.target.id, 10))
        }
        else {
          this.crossArray.push(parseInt(event.target.id, 10))
        }

        for (let key in vars.winPositions) {
          let noughts = 0
          let crosses = 0
          vars.winPositions[key].forEach(one => {
            if (this.noughtArray.includes(one)) {
              noughts++
              if (noughts === 3) {
                console.log('noughts win')
              }
            }
            if (this.crossArray.includes(one)) {
              crosses++
              if (crosses === 3) {
                console.log('crosses win')
              }
            }
          })
        }
      }
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

td
  width 100px
  height 100px

.board
  width 300px
  height 300px
  background-color $green-14
  margin-top 25px
  margin-left 25px
  display inline-table

.figure
  display block
  width 100%

.up-panel
  width 350px
  margin auto
  margin-bottom 20px
  display flex
  justify-content space-between

.down-panel
  width 350px
  margin auto
  margin-top 20px

.player-figure
  height 24px
  width 24px
</style>

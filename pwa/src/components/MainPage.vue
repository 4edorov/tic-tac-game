<template>
  <div class='layout-padding'>
    <div class='up-row'>
      <q-btn small class='reset-btn shadow-7' color='deep-orange' @click='resetGame()'>
        <q-icon name='refresh' />
      </q-btn>
    </div>
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
import { Dialog } from 'quasar'

export default {
  data () {
    return {
      firstPlayer: '1',
      numberOfPlayers: '1',
      firstScore: 0,
      secondScore: 0,
      state: JSON.parse(JSON.stringify(vars.initialState)),
      firstIsActive: true,
      noughtArray: [],
      crossArray: [],
      winMessage: '',
      autoStepsNumber: 0,
      prevStep: null,
      autoNoughtStrategy: null
    }
  },
  watch: {
    firstPlayer: function () {
      if (this.noughtArray.length === 0 && this.crossArray.length === 0) {
        this.firstIsActive = this.firstPlayer === '1'
      }
      if (this.numberOfPlayers === '1' && this.firstPlayer !== '1') {
        this.autoPushSquare()
      }
    },
    numberOfPlayers: function () {
      if (this.numberOfPlayers === '1' && this.firstPlayer !== '1') {
        this.autoPushSquare()
      }
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
  created () {
    if (this.numberOfPlayers === '1' && this.firstPlayer !== '1') {
      this.autoPushSquare()
    }
  },
  methods: {
    pushSquare (event) {
      this.prevStep = event.target.id

      if (!this.state[event.target.id].isActive) {
        this.state[event.target.id].isActive = !this.state[event.target.id].isActive
        if ((this.firstPlayer === '0' && this.firstIsActive) || (this.firstPlayer === '1' && !this.firstIsActive)) {
          this.state[event.target.id].figureSource = '../statics/nought.png'
        }
        if ((this.firstPlayer === '1' && this.firstIsActive) || (this.firstPlayer === '0' && !this.firstIsActive)) {
          this.state[event.target.id].figureSource = '../statics/cross.png'
        }

        if (this.state[event.target.id].figureSource === '../statics/nought.png') {
          this.noughtArray.push(parseInt(event.target.id, 10))
        }
        else {
          this.crossArray.push(parseInt(event.target.id, 10))
        }

        for (let key in vars.winPositions) {
          let crosses = 0
          vars.winPositions[key].forEach(one => {
            if (this.crossArray.includes(one)) {
              crosses++
              if (crosses === 3) {
                this.firstPlayer === '0' ? this.secondScore++ : this.firstScore++

                this.winMessage = 'Crosses are winner!'
                this.showRoundsActions()
              }
            }
          })
        }

        for (let key in vars.winPositions) {
          let noughts = 0
          vars.winPositions[key].forEach(one => {
            if (this.noughtArray.includes(one)) {
              noughts++
              if (noughts === 3) {
                this.firstPlayer === '0' ? this.firstScore++ : this.secondScore++

                this.winMessage = 'Noughts are winner!'
                this.showRoundsActions()
              }
            }
          })
        }

        if (!this.winMessage) {
          let sumSteps = this.crossArray.length + this.noughtArray.length
          if (sumSteps === 9) {
            this.winMessage = 'Draw!'
            this.showRoundsActions()
          }
        }

        this.firstIsActive = !this.firstIsActive

        if (this.numberOfPlayers === '1' && this.firstIsActive !== true && !this.winMessage) {
          this.autoPushSquare()
        }
      }
    },
    showRoundsActions () {
      Dialog.create({
        title: this.winMessage,
        buttons: [
          {
            label: 'Reset Game',
            color: 'negative',
            handler: () => {
              this.resetGame()
            }
          },
          {
            label: 'Keep Going',
            color: 'positive',
            handler: () => {
              this.toNextRound()
            }
          }
        ]
      })
    },
    toNextRound () {
      this.state = JSON.parse(JSON.stringify(vars.initialState))
      this.noughtArray = []
      this.crossArray = []
      this.firstIsActive = this.firstPlayer === '1'
      this.autoStepsNumber = 0
      this.winMessage = ''
      this.autoNoughtStrategy = null
      if (this.numberOfPlayers === '1' && this.firstPlayer !== '1') {
        this.autoPushSquare()
      }
    },
    resetGame () {
      this.firstPlayer = '1'
      this.numberOfPlayers = '0'
      this.firstScore = 0
      this.secondScore = 0
      this.state = JSON.parse(JSON.stringify(vars.initialState))
      this.firstIsActive = true
      this.noughtArray = []
      this.crossArray = []
      this.autoStepsNumber = 0
      this.winMessage = ''
      this.autoNoughtStrategy = null
    },
    autoPushSquare () {
      const id = this.autoBrain()
      console.log('id', id)
      setTimeout(() => {
        this.pushSquare({target: {id}})
      }, 500)
    },
    autoBrain () {
      if (this.firstPlayer === '0') {
        if (this.autoStepsNumber === 0) {
          this.autoStepsNumber++
          return '4'
        }
        if (this.autoStepsNumber === 1) {
          if (this.noughtArray[0] === 0) {
            this.autoStepsNumber++
            return '8'
          }
          if (this.noughtArray[0] === 2) {
            this.autoStepsNumber++
            return '6'
          }
          if (this.noughtArray[0] === 8) {
            this.autoStepsNumber++
            return '0'
          }
          if (this.noughtArray[0] === 6) {
            this.autoStepsNumber++
            return '2'
          }
          if (this.noughtArray[0] === 1) {
            this.autoStepsNumber++
            const variation = Math.round(Math.random())
            const id = variation ? '6' : '8'
            return id
          }
          if (this.noughtArray[0] === 5) {
            this.autoStepsNumber++
            const variation = Math.round(Math.random())
            const id = variation ? '0' : '6'
            return id
          }
          if (this.noughtArray[0] === 7) {
            this.autoStepsNumber++
            const variation = Math.round(Math.random())
            const id = variation ? '0' : '2'
            return id
          }
          if (this.noughtArray[0] === 3) {
            this.autoStepsNumber++
            const variation = Math.round(Math.random())
            const id = variation ? '2' : '8'
            return id
          }
        }
        if (this.autoStepsNumber >= 2) {
          let restCrossSquares, restNoughtSquares, crossIndex, noughtIndex

          for (let key in vars.winPositions) {
            restCrossSquares = vars.winPositions[key].slice()
            this.crossArray.forEach(one => {
              crossIndex = restCrossSquares.findIndex(winOne => {
                return one === winOne
              })
              if (crossIndex >= 0) {
                restCrossSquares.splice(crossIndex, 1)
              }
            })
            if (restCrossSquares.length === 1) {
              const id = restCrossSquares[0].toString()
              if (!this.state[id].isActive) {
                this.autoStepsNumber++
                return id
              }
            }
          }

          for (let key in vars.winPositions) {
            restNoughtSquares = vars.winPositions[key].slice()
            this.noughtArray.forEach(one => {
              noughtIndex = restNoughtSquares.findIndex(winOne => {
                return one === winOne
              })
              if (noughtIndex >= 0) {
                restNoughtSquares.splice(noughtIndex, 1)
              }
            })
            if (restNoughtSquares.length === 1) {
              const id = restNoughtSquares[0].toString()
              if (!this.state[id].isActive) {
                this.autoStepsNumber++
                return id
              }
            }
          }
        }

        return (this.getFarStepsArray(this.prevStep)[1] || this.getFarStepsArray(this.prevStep)[0]).id
      }
      else {
        if (this.autoStepsNumber >= 0) {
          let restCrossSquares, restNoughtSquares, crossIndex, noughtIndex

          for (let key in vars.winPositions) {
            restNoughtSquares = vars.winPositions[key].slice()
            this.noughtArray.forEach(one => {
              noughtIndex = restNoughtSquares.findIndex(winOne => {
                return one === winOne
              })
              if (noughtIndex >= 0) {
                restNoughtSquares.splice(noughtIndex, 1)
              }
            })
            if (restNoughtSquares.length === 1) {
              const id = restNoughtSquares[0].toString()
              if (!this.state[id].isActive) {
                this.autoStepsNumber++
                return id
              }
            }
          }

          for (let key in vars.winPositions) {
            restCrossSquares = vars.winPositions[key].slice()
            this.crossArray.forEach(one => {
              crossIndex = restCrossSquares.findIndex(winOne => {
                return one === winOne
              })
              if (crossIndex >= 0) {
                restCrossSquares.splice(crossIndex, 1)
              }
            })
            if (restCrossSquares.length === 1) {
              const id = restCrossSquares[0].toString()
              if (!this.state[id].isActive) {
                this.autoStepsNumber++
                return id
              }
            }
          }
        }

        if (this.autoStepsNumber === 0) {
          if (this.prevStep === '4') {
            this.autoNoughtStrategy = 'center'
            this.autoStepsNumber++

            return this.getNoughtSquareForCenter()
          }

          if (this.prevStep === '0' || this.prevStep === '2' || this.prevStep === '6' || this.prevStep === '8') {
            this.autoNoughtStrategy = 'corner'
            this.autoStepsNumber++

            return '4'
          }

          if (this.prevStep === '1' || this.prevStep === '3' || this.prevStep === '5' || this.prevStep === '7') {
            this.autoNoughtStrategy = 'side'
            this.autoStepsNumber++

            return '4'
          }
        }

        if (this.autoNoughtStrategy === 'center') {
          this.autoStepsNumber++
          return this.getNoughtSquareForCenter() || Object.keys(this.state).find(key => !this.state[key].isActive)
        }

        if (this.autoNoughtStrategy === 'corner' && this.autoStepsNumber === 1) {
          if (this.crossArray[0] === 0 || this.crossArray[0] === 8) {
            this.autoStepsNumber++
            const variation = Math.round(Math.random())
            let steps = ['2', '6']
            variation || steps.reverse()
            const id = steps.find(step => !this.state[parseInt(step, 10)].isActive)
            return id
          }
          if (this.crossArray[0] === 2 || this.crossArray[0] === 6) {
            this.autoStepsNumber++
            const variation = Math.round(Math.random())
            let steps = ['0', '8']
            variation || steps.reverse()
            const id = steps.find(step => !this.state[parseInt(step, 10)].isActive)
            return id
          }
        }

        if (this.autoNoughtStrategy === 'side' && this.autoStepsNumber === 1) {
          if (this.prevStep === '0') {
            this.autoStepsNumber++
            return '8'
          }
          if (this.prevStep === '2') {
            this.autoStepsNumber++
            return '6'
          }
          if (this.prevStep === '6') {
            this.autoStepsNumber++
            return '2'
          }
          if (this.prevStep === '8') {
            this.autoStepsNumber++
            return '0'
          }

          if (this.prevStep === '1') {
            if (this.crossArray[0] === 7) {
              this.autoStepsNumber++
              return Object.keys(this.state).find(key => !this.state[key].isActive)
            }
            if (this.crossArray[0] === 3) {
              this.autoStepsNumber++
              return '0'
            }
            if (this.crossArray[0] === 5) {
              this.autoStepsNumber++
              return '2'
            }
          }

          if (this.prevStep === '3') {
            if (this.crossArray[0] === 5) {
              this.autoStepsNumber++
              return Object.keys(this.state).find(key => !this.state[key].isActive)
            }
            if (this.crossArray[0] === 1) {
              this.autoStepsNumber++
              return '0'
            }
            if (this.crossArray[0] === 7) {
              this.autoStepsNumber++
              return '6'
            }
          }

          if (this.prevStep === '5') {
            if (this.crossArray[0] === 3) {
              this.autoStepsNumber++
              return Object.keys(this.state).find(key => !this.state[key].isActive)
            }
            if (this.crossArray[0] === 1) {
              this.autoStepsNumber++
              return '2'
            }
            if (this.crossArray[0] === 7) {
              this.autoStepsNumber++
              return '8'
            }
          }

          if (this.prevStep === '7') {
            if (this.crossArray[0] === 1) {
              this.autoStepsNumber++
              return Object.keys(this.state).find(key => !this.state[key].isActive)
            }
            if (this.crossArray[0] === 3) {
              this.autoStepsNumber++
              return '6'
            }
            if (this.crossArray[0] === 5) {
              this.autoStepsNumber++
              return '8'
            }
          }
        }

        if ((this.autoNoughtStrategy === 'corner' || this.autoNoughtStrategy === 'side') && this.autoStepsNumber >= 1) {
          this.autoStepsNumber++
          return Object.keys(this.state).find(key => !this.state[key].isActive)
        }
      }
    },
    getNoughtSquareForCenter () {
      const variation = Math.random()
      if ((variation <= 0.25) && !this.state[0].isActive) {
        return '0'
      }
      else if ((variation <= 0.5) && !this.state[2].isActive) {
        return '2'
      }
      else if ((variation <= 0.75) && !this.state[6].isActive) {
        return '6'
      }
      else if ((variation <= 1) && !this.state[8].isActive) {
        return '8'
      }
      else {
        return false
      }
    },
    getFarStepsArray (point) {
      let distancesArray = Object.keys(this.state).map(key => {
        if (!this.state[key].isActive) {
          return {dist: this.getDistance(point, key), id: key}
        }
      }).filter(one => one)
      distancesArray = distancesArray.sort((one, two) => two.dist - one.dist)
      return distancesArray
    },
    getDistance (point1, point2) {
      const pointCoordinates1 = vars.coordinatesTable[point1]
      const pointCoordinates2 = vars.coordinatesTable[point2]
      const distance = Math.sqrt(Math.pow(pointCoordinates1[1] - pointCoordinates1[0], 2) + Math.pow(pointCoordinates2[1] - pointCoordinates2[0], 2))
      return distance
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
  margin-bottom 12px
  display flex
  justify-content space-between

.reset-btn
  width 350px

.up-row
  text-align center
  margin-bottom 12px

.down-panel
  width 350px
  margin auto
  margin-top 20px

.player-figure
  height 24px
  width 24px
</style>

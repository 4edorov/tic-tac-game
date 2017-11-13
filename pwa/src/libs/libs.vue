<script>
const initialState = {
  0: {
    isActive: false,
    figureSource: ''
  },
  1: {
    isActive: false,
    figureSource: ''
  },
  2: {
    isActive: false,
    figureSource: ''
  },
  3: {
    isActive: false,
    figureSource: ''
  },
  4: {
    isActive: false,
    figureSource: ''
  },
  5: {
    isActive: false,
    figureSource: ''
  },
  6: {
    isActive: false,
    figureSource: ''
  },
  7: {
    isActive: false,
    figureSource: ''
  },
  8: {
    isActive: false,
    figureSource: ''
  }
}

const winPositions = {
  0: [0, 1, 2],
  1: [0, 3, 6],
  2: [1, 4, 7],
  3: [2, 5, 8],
  4: [2, 4, 6],
  5: [3, 4, 5],
  6: [6, 7, 8],
  7: [0, 4, 8]
}

module.exports = {
  initialState,
  winPositions
}
</script>

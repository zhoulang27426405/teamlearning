<template>
  <div id="wrap">
    <ul class="wrap-ul">
      <li v-for="(rows, i) in array" :key="i">
        <ul>
          <li
            v-for="(col, j) in rows"
            :key="j"
            :class="[
              'class-box',
              col ? (col === 2 ? 'color-red' : 'color-normal') : 'color-blank'
            ]"
          >
            {{ col }}
          </li>
        </ul>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  data: () => ({
    array: [],
    m: 20,
    n: 20,
    total: 0
  }),
  mounted () {
    this.init()
    this.findIsland()
  },
  methods: {
    init () {
      for (let i = 0; i < this.m; i++) {
        let temp = []
        for (let j = 0; j < this.n; j++) {
          // 0代表小岛，黑色显示，1代表水域，灰色显示
          temp.push(Math.round(Math.random()))
        }
        this.array.push(temp)
      }
    },
    check (x, y) {
      if (x === 0 || y === 0 || x === this.m - 1 || y === this.n - 1) {
        return false
      } else if (
        this.array[x - 1][y] === 1 &&
        this.array[x + 1][y] === 1 &&
        this.array[x][y - 1] === 1 &&
        this.array[x][y + 1] === 1
      ) {
        console.log(`第${x + 1}行，第${y + 1}列`)
        // 2代表符合条件的小岛，红色显示
        this.array[x][y] = 2
        this.total++
        return true
      } else {
        return false
      }
    },
    findIsland () {
      for (let i = 0; i < this.m; i++) {
        for (let j = 0; j < this.n; j++) {
          this.check(i, j)
        }
      }
      console.log(`总共${this.total}座小岛`)
    }
  }
}
</script>
<style scoped>
#wrap {
  width: 100%;
  text-align: center;
}
.wrap-ul {
  display: inline-block;
}
ul {
  font-size: 0;
  padding-left: 0;
}
li {
  list-style: none;
}
.class-box {
  display: inline-block;
  width: 28px;
  height: 28px;
  line-height: 28px;
  margin: 1px;
  text-align: center;
  font-size: 12px;
}
.color-normal {
  background: #eee;
}
.color-blank {
  background: #000;
  color: #fff;
}
.color-red {
  background: red;
}
</style>

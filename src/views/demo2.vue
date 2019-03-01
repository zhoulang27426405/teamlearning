<template>
  <div id="wrap">
    <div class="reset" @click="handleReset">重置</div>
    <ul :class="['wrap-ul', disable ? 'opacity' : '']">
      <li v-for="(rows, i) in array" :key="i">
        <ul>
          <li
            v-for="(col, j) in rows"
            :key="j"
            :class="['class-box', `color-${col}`]"
            @click="handleClick(i, j)"
          ></li>
        </ul>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  data: () => ({
    array: [],
    // 上一步棋的信息（位置和下棋人）
    status: [],
    disable: false,
    m: 20, // 行
    n: 20, // 列
    s: 2, // 人数
    l: 5 // 五子棋长度
  }),
  mounted () {
    this.init()
  },
  methods: {
    init () {
      this.array = []
      this.disable = false
      this.status = [
        {
          x: -1,
          y: -1,
          p: this.s
        }
      ]
      for (let i = 0; i < this.m; i++) {
        let temp = []
        for (let j = 0; j < this.n; j++) {
          temp.push(0)
        }
        this.array.push(temp)
      }
    },
    check () {
      let count1 = 0
      let count2 = 0
      let count3 = 0
      let count4 = 0

      let { x, y, p } = this.status[this.status.length - 1]
      for (let i = 0; i < 2 * this.l - 1; i++) {
        if (
          x - this.l + 1 + i >= 0 &&
          x - this.l + 1 + i < this.m - 1 &&
          this.array[x - this.l + 1 + i][y] === p
        ) {
          count1++
          if (count1 === this.l) {
            break
          }
        } else {
          count1 = 0
        }
      }

      for (let i = 0; i < 2 * this.l - 1; i++) {
        if (
          y - this.l + 1 + i >= 0 &&
          y - this.l + 1 + i < this.n - 1 &&
          this.array[x][y - this.l + 1 + i] === p
        ) {
          count2++
          if (count2 === this.l) {
            break
          }
        } else {
          count2 = 0
        }
      }

      for (let i = 0; i < 2 * this.l - 1; i++) {
        if (
          x - this.l + 1 + i >= 0 &&
          x - this.l + 1 + i < this.m - 1 &&
          y - this.l + 1 + i >= 0 &&
          y - this.l + 1 + i < this.n - 1 &&
          this.array[x - this.l + 1 + i][y - this.l + 1 + i] === p
        ) {
          count3++
          if (count3 === this.l) {
            break
          }
        } else {
          count3 = 0
        }
      }

      for (let i = 0; i < 2 * this.l - 1; i++) {
        if (
          x + this.l - 1 - i >= 0 &&
          x + this.l - 1 - i < this.m - 1 &&
          y - this.l + 1 + i >= 0 &&
          y - this.l + 1 + i < this.n - 1 &&
          this.array[x + this.l - 1 - i][y - this.l + 1 + i] === p
        ) {
          count4++
          if (count4 === this.l) {
            break
          }
        } else {
          count4 = 0
        }
      }

      return !![count1, count2, count3, count4].includes(this.l)
    },
    handleReset () {
      this.init()
    },
    handleClick (x, y) {
      if (this.disable) {
        return
      }
      if (this.array[x][y] === 0) {
        // 下一步棋，更改棋盘数组array，并记录这步棋的status（便于执行悔棋）
        this.array[x].splice(
          y,
          1,
          this.status[this.status.length - 1].p % this.s + 1
        )
        this.status.push({
          x,
          y,
          p: this.status[this.status.length - 1].p % this.s + 1
        })
        this.disable = this.check()
      } else {
        // 满足悔棋条件，开始悔棋
        if (
          this.status[this.status.length - 1].x === x &&
          this.status[this.status.length - 1].y === y
        ) {
          this.array[x].splice(y, 1, 0)
          this.status.pop()
        }
      }
    }
  }
}
</script>
<style scoped>
#wrap {
  width: 100%;
  text-align: center;
}
.opacity {
  opacity: .4;
}
.reset {
  display: inline-block;
  line-height: 1;
  white-space: nowrap;
  cursor: pointer;
  color: #fff;
  background-color: #409eff;
  border-color: #409eff;
  -webkit-appearance: none;
  text-align: center;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  outline: 0;
  margin: 20px auto;
  -webkit-transition: 0.1s;
  transition: 0.1s;
  font-weight: 500;
  padding: 9px 15px;
  font-size: 12px;
  border-radius: 3px;
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
.color-0 {
  background: #eee;
}
.color-1 {
  background: black;
}
.color-2 {
  background: red;
}
.color-3 {
  background: blue;
}
.color-4 {
  background: green;
}
</style>

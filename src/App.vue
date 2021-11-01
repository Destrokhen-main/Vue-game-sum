<template>
  <img id="img-logo" alt="Vue logo" src="./assets/logo.png" />
  <h3>Счёт: {{ score }}</h3>
  <h3 v-if="best != 0">Лучший счёт: {{ best }}</h3>
  <p><button @click="reload">Перезапустить</button></p>
  <div class="border">
    <div class="obj-m" @click="click()">Output</div>
    <div
      v-for="(item, index) in array"
      :key="index"
      :style="item.value == 'Output' ? 'font-size: 1rem' : ''"
      :class="'obj ' + (item.selected ? 'selected' : '')"
      @mousedown="pressDown(index)"
      @mouseup="pressUp(index)"
    >
      <l>{{ item.value != 0 ? item.value : "" }}</l>
    </div>
  </div>
</template>

<script>
function getValue() {
  let array = [1, 2, 4, 8, 16];
  let id = Math.floor(0 + Math.random() * (4 + 1 - 0));
  return array[id];
}

export default {
  name: "App",
  data() {
    let ar = [];
    for (let i = 0; i != 9; i++) {
      ar.push({
        value: 0,
        output: false,
        selected: false,
      });
    }
    return {
      array: ar,
      selected: null,
      score: 0,
      best: 0,
    };
  },
  methods: {
    click() {
      let chek = false;
      for (let i = 0; i != this.array.length; i++) {
        if (this.array[i].output != true && this.array[i].value == 0) {
          this.array[i].value = getValue();
          chek = true;
          break;
        }
      }
      if (!chek) {
        alert("Не пустых клеток");
      }
    },
    pressDown(index) {
      if (
        this.selected == null &&
        this.array[index].output != true &&
        this.array[index].value != 0
      ) {
        this.selected = {
          index,
          value: this.array[index].value,
        };

        this.array[index].selected = true;
      }
    },
    pressUp(index) {
      if (
        this.selected != null &&
        this.array[index].output != true &&
        this.selected.index != index &&
        this.array[index].value != 0
      ) {
        let obj = this.selected;
        let current = this.array[index];
        if (obj.value == current.value) {
          this.array[index].value += obj.value;
          this.score += obj.value * 2;
          this.array[obj.index].value = 0;
          this.array[obj.index].selected = false;
          this.selected = null;
        }
      } else if (
        this.selected != null &&
        this.selected.index != index &&
        this.array[index].value == 0
      ) {
        let obj = this.selected;
        this.array[index].value = obj.value;
        this.array[obj.index].value = 0;
        this.array[obj.index].selected = false;
        this.selected = null;
      } else if (this.selected != null && this.selected.index == index) {
        let obj = this.selected;
        this.array[obj.index].selected = false;
        this.selected = null;
      }
    },
    reload() {
      if (this.score > this.best) {
        this.best = this.score;
        let ar = [];
        for (let i = 0; i != 9; i++) {
          ar.push({
            value: 0,
            output: false,
            selected: false,
          });
        }
        this.array = ar;
        this.score = 0;
      }
    },
  },
};
</script>

<style>
#img-logo {
  width: 50px;
}

body {
  -moz-user-select: none;
  -khtml-user-select: none;
  user-select: none;
}

.obj {
  width: 100px;
  height: 100px;
  border: 1px solid rgb(187, 187, 187);
  background-color: rgb(245, 245, 245);
  font-size: 2rem;
  word-wrap: break-word;
  display: flex;
  align-items: center;
  text-align: center;
  justify-content: center;
  margin: auto;
  border-radius: 8px;
  margin: 1px;
}

.obj-m {
  height: 100px;
  width: 100%;
  font-size: 2rem;
  word-wrap: break-word;
  display: flex;
  align-items: center;
  text-align: center;
  justify-content: center;
  border-radius: 8px;
  background-color: rgb(222, 222, 222);
  margin-bottom: 2px;
}

.border {
  display: flex;
  flex-wrap: wrap;
  width: 320px;
  margin: auto;
  justify-content: space-between;
}

.selected {
  background-color: rgb(227, 227, 227);
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

<template>
  <div id="app">
    <div class="viewport">
      <div class="card">
        <div class="card-title">
          Awarie <i class="fas fa-exclamation-triangle"></i>
        </div>
        <div v-if="outages.length == 0" class="empty">
          Brak bierzacych awarii
        </div>
        <div v-else class="holder">
          <table>
            <tr>
              <th>Miejscowości</th>
              <th>Zakończenie</th>
            </tr>
            <tr v-for="(item, index) in outages" :key="index" class="message">
              <th>{{ item.place }}</th>
              <th>{{ item.timeRemaining }}</th>
            </tr>
          </table>
        </div>
      </div>
      <div class="card">
        <div class="card-title">Planowane <i class="far fa-clipboard"></i></div>
        <div class="holder">
          <table>
            <tr>
              <th>Rozpoczęcie</th>
              <th>Miejscowości</th>
              <th>Zakończenie</th>
            </tr>
            <tr v-for="(item, index) in planned" :key="index" class="message">
              <th>{{ item.startIn }}</th>
              <th>{{ item.place }}</th>
              <th>{{ item.timeRemaining }}</th>
            </tr>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss">
body {
  margin: 0;
  background-color: rgb(221, 221, 221);
  font-family: "Roboto", sans-serif;
}
#app {
  overflow: none;
}
.viewport {
  display: flex;
  gap: 10px;
  box-sizing: border-box;
  width: 100%;
  padding: 5px;
}

.card {
  background-color: whitesmoke;
  width: 80%;
  padding: 15px;
  height: 92vh;
}

.card-title {
  font-size: 30px;
}

.message {
  border-top: 1px solid black;
}

.message:hover {
  background-color: rgb(138, 138, 138);
}
.holder {
  height: 90%;
  overflow: auto;
  box-sizing: border-box;
}
</style>

<script>
import axios from "axios";
export default {
  data() {
    return {
      planned: [],
      outages: [],
    };
  },
  methods: {
    getData() {
      let that = this;
      axios
        .get("http://localhost:7777/data")
        .then(function (res) {
          that.fetchData = res.data;
          that.planned = res.data.planned;
          that.outages = res.data.outages;
          that.planned.sort((a, b) => (a.unix > b.unix ? 1 : -1));
          that.outages.sort((a, b) => (a.unix > b.unix ? 1 : -1));
        })
        .catch(function (e) {
          console.log(e);
        });
    },
  },
  mounted() {
    this.getData();
    setInterval(this.getData, 100);
  },
};
</script>


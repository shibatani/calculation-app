<template>
  <div id="app">
    <CalcForm 
      @result="appAction" 
    />
    <hr />
    <div>
      <table v-html="dataLog"></table>
    </div>
  </div>
</template>


<script>
import CalcForm from "./components/CalcForm.vue";

export default {
  name: "app",
  components: {
    CalcForm,
  },
  data: function () {
    return {
      result: [],
    };
  },
  computed: {
    dataLog: function () {
      let table =
        '<tr><th class="head">Expression</th><th class="head">Value</th></tr>';
      for (const i in this.result) {
        table +=
          "<tr><td>" +
          this.result[i][0] +
          "</td><th>" +
          this.result[i][1] +
          "</th></tr>";
      }
      return table;
    },
  },
  created: function () {
    const items = localStorage.getItem("log");
    const logs = JSON.parse(items);
    if (logs != null) {
      this.result = logs;
    }
  },
  methods: {
    appAction: function (exp, res) {
      this.result.unshift([exp, res]);
      if (this.result.length > 10) {
        this.result.pop();
      }
      const log = JSON.stringify(this.result);
      localStorage.setItem("log", log);
    },
  },
};
</script>


<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin: 5px;
}
tr td {
  padding: 5px;
  border: 1px solid gray;
}
tr th {
  padding: 5px;
  border: 1px solid gray;
}
tr th.head {
  background-color: black;
  color: white;
}
</style>
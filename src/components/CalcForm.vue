<template>
  <div class="hello">
    <h1>CALC</h1>
    <p>{{ message }}</p>
    <hr />
    <div>
      <div>
        <textarea 
          v-model="fomula" 
          cols="40" 
          rows="5"
          placeholder="計算式を入力してください"
        >
        </textarea>
      </div>
      <div>
        <button @click="calculate">
          CALC
        </button>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: "CalcForm",
  data: function () {
    return {
      message: "Enter expression:",
      fomula: "",
    };
  },
  methods: {
    calculate: function () {
      const arr = this.fomula.trim().split("\n");
      const last = arr.pop();
      let fn = "";
      for (const n in arr) {
        if (arr[n].trim() != "") {
          fn += "const " + arr[n] + ";";
        }
      }
      fn += "return " + last + ";";
      const exp = "function f(){" + fn + "} f();";
      const ans = eval(exp);
      this.message = "answer: " + ans;
      let re = arr.join(";").trim();
      if (re != "") {
        re += ";";
      }
      re += last;
      this.$emit("result", re, ans);
    },
  },
};
</script>
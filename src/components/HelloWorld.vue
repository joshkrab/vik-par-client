<template>
  <div class="wrapper">
    <v-sheet class="mx-auto">
      <v-form @submit.prevent>
        <v-text-field
          class="inputV"
          v-model="firstUrl"
          label="Input URL"
        ></v-text-field>
        <v-text-field
          class="inputV"
          v-model="pageCount"
          label="How many pages?"
        ></v-text-field>
        <v-btn @click="getData" type="submit" block class="mt-2">Submit</v-btn>
      </v-form>
      <div class="out-block">{{ output }}</div>
    </v-sheet>
  </div>
</template>

<script>
import axios from "axios";
import exportFromJSON from "export-from-json";

export default {
  components: {},
  data() {
    return {
      firstUrl: null,
      pageCount: null,
      output: "",
    };
  },

  methods: {
    async getData() {
      this.output = '';
      if (!this.firstUrl) {
        alert("Input url!");
        return;
      }

      const { data } = await axios.post("http://localhost:8080/api/vikpar", {
        url: this.firstUrl,
        pageCount: this.pageCount || 1,
      });

      data
        ? (this.output = "Successfully")
        : (this.output = "Something wrong...");

      const fileName = "np-data";
      const exportType = exportFromJSON.types.xls;

      if (data) {
        console.log(data);
        exportFromJSON({data, fileName, exportType });
      }
      return;
    },
  },
};
</script>

<style>
.out-block {
  height: 40px;
  margin: 10px;
  text-align: center;
}
</style>

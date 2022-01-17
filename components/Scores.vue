<template>
  <div class="container">
    <div class="row mb-5">
      <div class="col-12">
        <h1>Copy/paste the scrores below</h1>
      </div>
      <div class="col-12">
        Quarter
        <select name="quarter" v-model="quarter">
          <option value="1">1</option>
          <option value="2">2</option>
          <option value="3">3</option>
          <option value="4">4</option>
        </select>
      </div>
      <div class="col-12">
        <textarea
          name="testing"
          id=""
          cols="130"
          rows="10"
          v-model="testing"
        ></textarea>
      </div>
      <div class="col-6">
        <button class="btn btn-primary" @click="process()">Process</button>
      </div>
      <div class="col-6">
        <button class="btn btn-secondary" @click="rawData()">
          Show raw data
        </button>
        <button class="btn">Compute grades</button>
      </div>
    </div>

    <div class="row" v-for="r in rawdata" :key="r.id">
      <div class="col-3">
        <h3>{{ r.attributes.name }}</h3>
      </div>
      <div class="col-9">
        <div class="row">
          <div class="col-12">
            <h4>Homework Scrores</h4>
          </div>
          <div class="col-2">
            <strong>Quarter</strong>
          </div>
          <div class="col-10">
            <strong>Grade</strong>
          </div>
          <template v-for="g in r.attributes.homework_scores.data">
            <div class="col-2">{{ g.attributes.quarter }}</div>
            <div class="col-10">{{ g.attributes.score }}</div>
          </template>
        </div>

        <div class="row">
          <div class="col-12">
            <h4>Test Scrores</h4>
          </div>
          <div class="col-2">
            <strong>Quarter</strong>
          </div>
          <div class="col-10">
            <strong>Grade</strong>
          </div>
          <template v-for="g in r.attributes.test_scores.data">
            <div class="col-2">{{ g.attributes.quarter }}</div>
            <div class="col-10">{{ g.attributes.score }}</div>
          </template>
        </div>
      </div>
        
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data: () => ({
    quarter: "1",
    testing: null,
    rawdata: null,
  }),
  methods: {
    async process() {
      try {
        let proc = await this.$http.$post("students/post-scores", {
          quarter: this.quarter,
          testing: this.testing,
        });
      } catch (error) {
        console.log(error);
      }

      console.log(this.quarter, this.testing);
      alert("Scores are saved.");
    },
    async rawData() {
      let rawdata = await this.$http.$get(
        "students?populate[0]=homework_scores&populate[1]=test_scores"
      );
      this.rawdata = rawdata.data;

      console.log(rawdata);
    },
  },
};
</script>

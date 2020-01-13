<template>
  <div class="addRun">
    <div class="container">
      <h1>Add Run</h1>

      <form @submit.prevent="addRun()">
        <div class="form-group row">
          <label for="txtTitle">Exercice Title:</label>
          <input
            type="text"
            class="form-control"
            id="txtTitle"
            placeholder="Enter Title"
            v-model="title"
          />
        </div>
        <div class="form-group row">
          <label for="txtKm">Distance in killometers:</label>
          <input
            type="number"
            class="form-control"
            id="txtKm"
            placeholder="Distance"
            v-model="dist"
          />
        </div>

        <div class="form-group row">
          <label for="txtStart">Start Point:</label>
          <input
            type="text"
            class="form-control"
            id="txtStart"
            placeholder="Start"
            v-model="startPoint"
          />
        </div>
        <div class="form-group row">
          <label for="txtEnd">End Point:</label>
          <input
            type="text"
            class="form-control"
            id="txtEnd"
            placeholder="Finish"
            v-model="endPoint"
          />
        </div>

        <div class="form-group row">
          <label for="dataTimeStart">Start Time:</label>
          <input
            type="time"
            step="1"
            min="01:00"
            max="24:00"
            class="form-control"
            id="timeStart"
            placeholder="start time"
            v-model="startTime"
          />
        </div>

        <div class="form-group row">
          <label for="timeEnd">End Time:</label>
          <input
            type="time"
            step="1"
            min="01:00"
            max="24:00"
            class="form-control"
            id="timeEnd"
            placeholder="end time"
            v-model="endTime"
          />
        </div>

        <div class="form-group row">
          <label for="txtDate">Date:</label>
          <input type="date" class="form-control" id="txtTitle" placeholder="date" v-model="date" />
        </div>
        <button type="submit" class="btn btn-primary">Submit</button>
      </form>
    </div>
  </div>
</template>
<script>
import { mapGetters } from "vuex";
import { mapMutations } from "vuex";
// import { start } from "repl";
export default {
  data() {
    return {
      title: "",
      dist: 0,
      startPoint: "",
      endPoint: "",
      startTime: Date.now(),
      endTime: Date.now(),
      date: Date.now(),
      confirmDateFail: false
    };
  },
  computed: {
    ...mapGetters({
      getRunnings: "getRunnings",
      getRunningsLastId: "getRunningsLastId",
      getRunByDate: "getRunByDate",
      getElementByTitle: "getElementByTitle"
    }),

    ...mapMutations({
      ADD_RUNNING: "ADD_RUNNING"
      // getRunnings: "getRunnings"
    })
  },

  methods: {
    addRun() {
      this.confirmTime();
      alert(this.title);
      if (this.getElementByTitle(this.title)) {
        alert("Chose an other title");
      } else {
        if (
          this.getRunByDate(this.startTime, this.date).length > 0 ||
          this.confirmDateFail
        ) {
          alert("Cannod add this run in this date and time");
        } else {
          this.$store.commit("ADD_RUNNING", {
            id: this.getRunningsLastId,
            title: this.title,
            dist: this.dist,
            startPoint: this.startPoint,
            endPoint: this.endPoint,
            startTime: this.startTime,
            endTime: this.endTime,
            date: this.date
          });
        }
      }
    },

    confirmTime() {
      if (this.startTime > this.endTime) {
        this.confirmDateFail = true;
        alert("li");
      } else {
        this.confirmDateFail = false;
      }
    }
  }
};
</script>

<style scoped>
</style>

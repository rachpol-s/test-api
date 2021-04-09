<template>
  <div class="bg-light">
    <div class="container">
      <div class="row">
        <div
          class="btn-group"
          role="group"
          style="width:100%; padding-top:30px"
        >
          <button
            type="button"
            class="btn"
            :class="{ buttonblue: btn === 1 }"
            @click="btn = 1"
          >
            All
          </button>
          <button
            type="button"
            class="btn"
            :class="{ buttonblue: btn === 2 }"
            @click="btn = 2"
          >
            Launched
          </button>
          <button
            type="button"
            class="btn"
            :class="{ buttonblue: btn === 3 }"
            @click="btn = 3"
          >
            Upcoming
          </button>
        </div>
      </div>

      <div v-if="btn === 1" class="row mt-2">
        <div
          class="card text-left mt-2"
          v-for="data in info"
          :key="data.index"
          style="width:100%"
        >
          <div class="card-body">
            <div class="row" style="padding:20px">
              <div v-if="data.links.patch.small !== null" class="icon">
                <img :src="data.links.patch.small" />
              </div>
              <div v-else class="icon" style="margin-right: 20px">
                <img src="https://imgur.com/BrW201S.png" />
              </div>
              <h4 class="card-title" style="margin-right: auto">
                {{ data.name }}
              </h4>
              <div v-if="data.crew.length" style="width:7%; margin-right: 20px">
                <p
                  class="text-center text-white"
                  style="background-color: blue; border-radius:15px"
                >
                  {{ data.crew.length }} crews
                </p>
              </div>
              <p style="margin-right: 20px">{{ new Date(data.date_local) }}</p>
              <p
                style="margin-right: 20px; color:blue"
                v-if="new Date() < new Date(data.date_local)"
              >
                Upcoming
              </p>
              <p style="margin-right: 20px; color:blue" v-else>Launched</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      info: "",
      info2: "",
      btn: 1,
      allCrewId: [],
      mapCrew: [
        {
          id: "",
          name: "",
        },
      ],
    };
  },
  watch: {
    info() {
      console.log("info1");
      this.info.forEach((element) => {
        if (element.crew.length > 0) {
          // console.log('crew id = '+element.crew)
          element.crew.forEach((element2) => {
            // console.log('crew id = '+element2)
            this.allCrewId.push(element2);
          });
        }
      });
      // console.log(this.allCrewId);

      // console.log(this.mapCrew);
      this.mapCrew.pop();
      // console.log(this.mapCrew);

      console.log("info2");
      this.info2.forEach((element) => {
        this.allCrewId.forEach((element2) => {
          console.log("main = " + element.id);
          console.log("check = " + element2);
          if (element.id === element2) {
            console.log(true);
            this.mapCrew.push({
              id: element.id,
              name: element.name,
            });
            // console.log(this.mapCrew);
          }
        });
      });
    },
  },
  computed: {},
  method: {},
  mounted() {
    axios
      .get("https://api.spacexdata.com/v4/launches")
      .then(
        (response) => ((this.info = response.data), console.log(this.info))
      );

    axios
      .get("https://api.spacexdata.com/v4/crew")
      .then(
        (response) => ((this.info2 = response.data), console.log(this.info2))
      );
  },
};
</script>

<style scoped>
button {
  background-color: whitesmoke;
  border: solid 1px;
  border-color: rgb(224, 221, 221);
}
.buttonblue {
  background-color: blue;
  color: white;
}
.buttonblue:hover {
  color: white;
}
.icon img {
  width: 30%;
}</style
>>

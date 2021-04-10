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
            class="btn col"
            :class="{ buttonblue: btn === 1 }"
            @click="btn = 1"
          >
            All
          </button>
          <button
            type="button"
            class="btn col"
            :class="{ buttonblue: btn === 2 }"
            @click="btn = 2"
          >
            Launched
          </button>
          <button
            type="button"
            class="btn col"
            :class="{ buttonblue: btn === 3 }"
            @click="btn = 3"
          >
            Upcoming
          </button>
        </div>
      </div>

      <div v-if="btn === 1" class="row mt-2">
        <div>
          <div
            class="card mt-2"
            v-for="data in apiLaunch"
            :key="data.index"
            style="width:100%"
          >
            <div class="card-body">
              <div class="row align-items-center" style="padding:20px">
                <div class="col-1 icon">
                  <img
                    :src="data.links.patch.small"
                    v-if="data.links.patch.small !== null"
                  />
                  <img src="https://imgur.com/BrW201S.png" v-else />
                </div>

                <div class="col-3">
                  <h4>
                    {{ data.name }}
                  </h4>
                </div>

                <div class="col-6">
                  <div class="row justify-content-end">
                    <div style="margin-right:10px">
                      <p
                        v-if="data.crew.length"
                        class="text-center text-white"
                        style="background-color: blue; border-radius:20px; padding:5px"
                      >
                        {{ data.crew.length }} crews
                      </p>
                    </div>

                    <p>
                      {{ new Date(data.date_local) }}
                    </p>
                  </div>
                </div>

                <div class="col-2 text-right blue">
                  <p v-if="new Date() < new Date(data.date_local)">
                    Upcoming
                  </p>
                  <p v-else>Launched</p>
                </div>
              </div>
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
      apiLaunch: "",
      apiCrew: "",
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
    apiLaunch() {
      this.apiLaunch.forEach((element) => {
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
      this.apiCrew.forEach((element) => {
        this.allCrewId.forEach((element2) => {
          // console.log("main = " + element.id);
          // console.log("check = " + element2);
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
        (response) => (
          (this.apiLaunch = response.data), console.log(this.apiLaunch)
        )
      );

    axios
      .get("https://api.spacexdata.com/v4/crew")
      .then(
        (response) => (
          (this.apiCrew = response.data), console.log(this.apiCrew)
        )
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
  width: 100%;
}
.blue{
  color: blue;
}
</style>

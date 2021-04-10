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
        <div v-for="data in apiLaunch" :key="data.index" style="width:100%">
          <div
            class="card mt-2"
            @click="
              showModal(
                data.name,
                data.date_local,
                data.crew,
                data.rocket,
                data.launchpad
              )
            "
          >
            <div class="card-body">
              <div class="row align-items-center" style="padding:20px">
                <div class="col-2 icon">
                  <img
                    :src="data.links.patch.small"
                    v-if="data.links.patch.small !== null"
                  />
                  <img
                    src="@/assets/default-image-300x300.jpg"
                    v-else
                    class="default-image"
                  />
                </div>

                <div class="col-5">
                  <h4>
                    {{ data.name }}
                  </h4>
                </div>

                <div class="col-5">
                  <div class="row justify-content-end align-items-center">
                    <div>
                      <p
                        v-if="data.crew.length"
                        class="text-center text-white crew"
                      >
                        {{ data.crew.length }} crews
                      </p>
                    </div>

                    <p style="padding-left:20px">
                      {{ new Date(data.date_local).toString().slice(0, 15) }}
                    </p>
                    <div class="text-right blue" style="padding-left:20px">
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

      <div v-if="btn === 2" class="row mt-2">
        <div v-for="data in apiLaunch" :key="data.index" style="width:100%">
          <div v-if="new Date() > new Date(data.date_local)">
            <div
              class="card mt-2"
              @click="
                showModal(
                  data.name,
                  data.date_local,
                  data.crew,
                  data.rocket,
                  data.launchpad
                )
              "
            >
              <div class="card-body">
                <div class="row align-items-center" style="padding:20px">
                  <div class="col-2 icon">
                    <img
                      :src="data.links.patch.small"
                      v-if="data.links.patch.small !== null"
                    />
                    <img
                      src="@/assets/default-image-300x300.jpg"
                      v-else
                      class="default-image"
                    />
                  </div>

                  <div class="col-5">
                    <h4>
                      {{ data.name }}
                    </h4>
                  </div>

                  <div class="col-5">
                    <div class="row justify-content-end align-items-center">
                      <div>
                        <p
                          v-if="data.crew.length"
                          class="text-center text-white crew"
                        >
                          {{ data.crew.length }} crews
                        </p>
                      </div>

                      <p style="padding-left:20px">
                        {{ new Date(data.date_local).toString().slice(0, 15) }}
                      </p>
                      <div class="text-right blue" style="padding-left:20px">
                        <p>
                          Launched
                        </p>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div v-if="btn === 3" class="row mt-2">
        <div v-for="data in apiLaunch" :key="data.index" style="width:100%">
          <div v-if="new Date() < new Date(data.date_local)">
            <div
              class="card mt-2"
              @click="
                showModal(
                  data.name,
                  data.date_local,
                  data.crew,
                  data.rocket,
                  data.launchpad
                )
              "
            >
              <div class="card-body">
                <div class="row align-items-center" style="padding:20px">
                  <div class="col-2 icon">
                    <img
                      :src="data.links.patch.small"
                      v-if="data.links.patch.small !== null"
                    />
                    <img
                      src="@/assets/default-image-300x300.jpg"
                      v-else
                      class="default-image"
                    />
                  </div>

                  <div class="col-5">
                    <h4>
                      {{ data.name }}
                    </h4>
                  </div>

                  <div class="col-5">
                    <div class="row justify-content-end align-items-center">
                      <div>
                        <p
                          v-if="data.crew.length"
                          class="text-center text-white crew"
                        >
                          {{ data.crew.length }} crews
                        </p>
                      </div>

                      <p style="padding-left:20px">
                        {{ new Date(data.date_local).toString().slice(0, 15) }}
                      </p>
                      <div class="text-right blue" style="padding-left:20px">
                        <p>
                          Upcoming
                        </p>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div>
        <b-modal ref="my-modal" hide-footer hide-header>
          <div class="text-center">
            <h3>
              {{ modalItem.name }}
            </h3>
            <p>
              {{ new Date(modalItem.date).toString().slice(0, 24) }}
            </p>
            <hr />
            <div class="row justify-content-center">
              <div class="col-3">
                <div v-if="modalItem.crewItem.crew_name.length > 0">
                  <p class="text-white crew">
                    {{ modalItem.crewItem.crew_name.length }} crews
                  </p>
                </div>
                <div v-else>
                  <p class="text-white crew">
                    No crew
                  </p>
                </div>
              </div>
            </div>
            <div class="row">
              <div
                class="col"
                v-for="data in modalItem.crewItem.crew_img"
                :key="data.index"
              >
                <img class="img-fluid avatar" :src="data" />
              </div>
            </div>
            <div class="row">
              <div
                class="col"
                v-for="data in modalItem.crewItem.crew_name"
                :key="data.index"
              >
                <p>{{ data }}</p>
              </div>
            </div>
            <hr />
            <div class="row justify-content-center">
              <div class="col-3">
                <p class="text-white crew">
                  Rocket
                </p>
              </div>
            </div>
            <div class="row justify-content-center">
              <div class="col">
                <h4>
                  {{ modalItem.rocket }}
                </h4>
              </div>
            </div>
            <div v-for="data in modalItem.rocket_img" :key="data.index">
              <img class="img-fluid" :src="data" style="padding-bottom:10px" />
            </div>
            <hr />
            <div class="row justify-content-center">
              <div class="col-3">
                <p class="text-white crew">
                  Launchpad
                </p>
              </div>
            </div>
            <div class="row justify-content-center">
              <div class="col">
                <h4>
                  {{ modalItem.launchpad }}
                </h4>
              </div>
            </div>
          </div>
          <b-button
            class="mt-3"
            variant="outline-danger"
            block
            @click="hideModal"
            >Close</b-button
          >
        </b-modal>
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
      apiLaunchpad: "",
      apiRocket: "",
      btn: 1,
      mapRocket: [],
      mapLaunchpad: [],
      modalItem: {
        name: "",
        date: "",
        crewItem: {
          crew_name: [],
          crew_img: [],
        },
        rocket: "",
        rocket_img: "",
        launchpad: "",
      },
    };
  },
  methods: {
    async showModal(name, date, crew_id, rocket_id, launchpad_id) {
      this.modalItem.crewItem.crew_name = [];
      this.modalItem.crewItem.crew_img = [];

      // console.log(name, date, crew_id, rocket_id, launchpad_id);

      if (crew_id.length > 0) {
        await crew_id.forEach((element) => {
          axios.get("https://api.spacexdata.com/v4/crew/" + element).then(
            (response) => (
              (this.apiCrew = response.data),
              // console.log(this.apiCrew),
              this.modalItem.crewItem.crew_img.push(this.apiCrew.image),
              this.modalItem.crewItem.crew_name.push(this.apiCrew.name)
            )
          );
        });
      }

      await axios
        .get("https://api.spacexdata.com/v4/rockets/" + rocket_id)
        .then(
          (response) => (this.apiRocket = response.data)
          // console.log(this.apiRocket)
        );

      await axios
        .get("https://api.spacexdata.com/v4/launchpads/" + launchpad_id)
        .then(
          (response) => (this.apiLaunchpad = response.data)
          // console.log(this.apiLaunchpad)
        );

      this.modalItem.name = name;
      this.modalItem.date = date;
      this.modalItem.rocket = this.apiRocket.name;
      this.modalItem.launchpad = this.apiLaunchpad.name;
      this.modalItem.rocket_img = this.apiRocket.flickr_images;

      // console.log(this.modalItem);
      this.$refs["my-modal"].show();
    },
    hideModal() {
      this.$refs["my-modal"].hide();
    },
  },

  computed: {},
  watch: {},

  mounted() {
    axios.get("https://api.spacexdata.com/v4/launches").then(
      (response) => (this.apiLaunch = response.data)
      // console.log(this.apiLaunch)
    );

    axios.get("https://api.spacexdata.com/v4/crew").then(
      (response) => (this.apiCrew = response.data)
      // console.log(this.apiCrew)
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
  background-color: rgb(10, 10, 173);
  color: white;
}
.buttonblue:hover {
  color: white;
}
.icon img {
  width: 60%;
}
.crew {
  background-color: rgb(10, 10, 173);
  border-radius: 20px;
  padding-left: 10px;
  padding-right: 10px;
}
.blue {
  color: rgb(10, 10, 173);
}
.default-image {
  border-radius: 50%;
}
.avatar {
  vertical-align: middle;
  width: 50px;
  height: 50px;
  border-radius: 50%;
}
</style>

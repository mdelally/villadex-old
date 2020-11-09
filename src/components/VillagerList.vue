<template>
  <div class="mt-8 bg-yellow-500 p-8 rounded-lg">
    <div class="text-center p-16 text-5xl text-yellow-700" v-if="loading">
      {{ loadingText }}
    </div>

    <div
      v-if="!loading"
      class="p-4 bg-yellow-300 rounded-lg text-yellow-700 sticky top-0 z-50 mb-2 border-2 border-yellow-600 flex justify-between items-center"
    >
      <div class="flex flex-col items-center">
        <label class="text-sm font-black">FILTER BY NAME</label>
        <input
          type="text"
          class="p-2 rounded-full bg-yellow-200 border-2 border-yellow-400"
          v-model="nameFilter"
        />
      </div>

      <div class="toolbar flex">
        <div class="flex flex-col items-center">
          <label for="personality" class="text-sm font-black"
            >PERSONALITY</label
          >
          <select
            name="personality"
            class="px-4 py-2 rounded-full bg-yellow-200 border-2 border-yellow-400"
            v-model="personality"
          >
            <option :value="null" default>-- Personality --</option>
            <option value="Cranky">Cranky</option>
            <option value="Jock">Jock</option>
            <option value="Lazy">Lazy</option>
            <option value="Normal">Normal</option>
            <option value="Peppy">Peppy</option>
            <option value="Sisterly">Sisterly</option>
            <option value="Smug">Smug</option>
            <option value="Snooty">Snooty</option>
          </select>
        </div>

        <div class="flex flex-col ml-2 items-center">
          <label for="personality" class="text-sm font-black">HOBBY</label>
          <select
            name="personality"
            class="px-4 py-2 rounded-full bg-yellow-200 border-2 border-yellow-400"
            v-model="hobby"
          >
            <option :value="null" default>-- Hobby --</option>
            <option value="Education">Education</option>
            <option value="Fashion">Fashion</option>
            <option value="Fitness">Fitness</option>
            <option value="Music">Music</option>
            <option value="Nature">Nature</option>
            <option value="Play">Playing</option>
          </select>
        </div>
      </div>
    </div>

    <div v-if="!loading && filteredVillagers.length > 0">
      <transition-group
        tag="div"
        class="flex flex-wrap items-center justify-center"
        mode="out-in"
        name="cards"
      >
        <VillagerCard v-for="v in filteredVillagers" :key="v.id" :v="v" />
      </transition-group>
    </div>

    <div class="text-center p-16 text-5xl text-yellow-700" v-else-if="!loading">
      NO VILLAGERS MATCH!
    </div>
  </div>
</template>

<script>
import axios from "axios";
import VillagerCard from "./VillagerCard.vue";

export default {
  name: "VillagerList",
  components: { VillagerCard },

  data() {
    return {
      villagers: [],
      loading: true,
      nameFilter: "",
      personality: null,
      hobby: null,
      loadingText: "LOADING...",
    };
  },

  computed: {
    filteredByName() {
      return this.villagers.filter((v) => {
        return v.name.toLowerCase().includes(this.nameFilter.toLowerCase());
      });
    },
    filteredVillagers() {
      let self = this;

      if (self.personality === null && self.hobby === null)
        return this.filteredByName;

      return this.filteredByName.filter((v) => {
        if (self.personality !== null && self.hobby !== null)
          return (
            v.personality === self.personality &&
            v.nh_details.hobby === self.hobby
          );

        if (self.personality !== null && self.hobby === null)
          return v.personality === this.personality;

        if (self.personality === null && self.hobby !== null)
          return v.nh_details.hobby === self.hobby;
      });
    },
  },

  methods: {
    getVillagers() {
      if (!!localStorage.getItem("villadex-villagers")) {
        this.villagers = JSON.parse(localStorage.getItem("villadex-villagers"));
        this.loading = false;
        return;
      }

      axios
        .get(
          "https://api.nookipedia.com/villagers?game=nh&nhdetails=true&thumbsize=160",
          {
            headers: {
              "X-API-KEY": "b08eeca9-ec87-4ede-80b8-73c630bb532f",
            },
          }
        )
        .then((results) => {
          this.villagers = results.data;

          localStorage.setItem(
            "villadex-villagers",
            JSON.stringify(results.data)
          );
          this.loading = false;
        })
        .catch((results) => {
          this.loadingText = "LOAD FAILED!";
        });
    },
  },

  mounted() {
    this.getVillagers();
  },
};
</script>

<style lang="scss">
div.villager-card-container {
  flex-basis: 25%;
}

.cards-enter-active,
.cards-leave-active {
  transition: all 0.5s ease-in-out;
}
.cards-enter, .cards-leave-to /* .cards-leave-active below version 2.1.8 */ {
  opacity: 0;
  transform: translateY(-50px);
}
</style>
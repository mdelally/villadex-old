<template>
  <div class="mt-8 bg-yellow-500 p-8 rounded-lg">
    <div class="text-center p-16 text-yellow-700" v-if="loading">
      <h3 class="text-5xl">Initial Loading...</h3>
      <p class="text-3xl">
        If this is the first time you're visiting Villadex, please allow the app
        to load all the Villager data. This process can take upwards of a minute
        or so depending on your connection. The data will be stored locally on
        your device to save you time in the future.
      </p>
    </div>

    <div
      v-if="!loading"
      class="p-4 bg-yellow-300 rounded-lg text-yellow-700 mb-2 border-2 border-yellow-600 flex justify-between items-center"
    >
      <div class="flex flex-col items-center">
        <label class="text-sm font-black">FILTER BY NAME</label>
        <input
          type="text"
          class="p-2 rounded-full bg-yellow-200 border-2 border-yellow-400 w-64 text-center"
          v-model="nameFilter"
        />
      </div>

      <div class="toolbar flex">
        <!-- SPECIES -->
        <div class="flex flex-col items-center">
          <label for="personality" class="text-sm font-black">SPECIES</label>
          <select
            name="personality"
            class="px-4 py-2 rounded-full bg-yellow-200 border-2 border-yellow-400"
            v-model="specie"
          >
            <option :value="null" default>All</option>
            <option v-for="s in species" :key="s" :value="s">
              {{ s }}
            </option>
          </select>
        </div>

        <!-- PERSONALITY -->
        <div class="flex flex-col ml-2 items-center">
          <label for="personality" class="text-sm font-black"
            >PERSONALITY</label
          >
          <select
            name="personality"
            class="px-4 py-2 rounded-full bg-yellow-200 border-2 border-yellow-400"
            v-model="personality"
          >
            <option :value="null" default>All</option>
            <option v-for="p in personalities" :key="p" :value="p">
              {{ p }}
            </option>
          </select>
        </div>

        <!-- HOBBY -->
        <div class="flex flex-col ml-2 items-center">
          <label for="personality" class="text-sm font-black">HOBBY</label>
          <select
            name="personality"
            class="px-4 py-2 rounded-full bg-yellow-200 border-2 border-yellow-400"
            v-model="hobby"
          >
            <option :value="null" default>All</option>
            <option v-for="h in hobbies" :key="h" :value="h">{{ h }}</option>
          </select>
        </div>

        <!-- CLEAR FILTERS -->
        <button
          class="text-yellow-700 px-6 font-bold"
          @click="clearFilters"
          v-if="hasFilters"
        >
          Clear Filters
        </button>
      </div>
    </div>

    <div v-if="!loading && filteredVillagers.length > 0">
      <transition-group
        tag="div"
        class="flex flex-wrap items-center justify-center"
        mode="out-in"
        name="cards"
      >
        <VillagerCard
          v-for="v in filteredVillagers"
          :key="v.id"
          :v="v"
          @select-villager="$emit('select-villager', v)"
        />
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
      specie: null,
      loadingText: "LOADING...",
      personalities: [
        "Cranky",
        "Jock",
        "Lazy",
        "Normal",
        "Peppy",
        "Sisterly",
        "Smug",
        "Snooty",
      ],
      hobbies: ["Education", "Fashion", "Fitness", "Music", "Nature", "Play"],
      species: [
        "Alligator",
        "Anteater",
        "Bear",
        "Bird",
        "Bull",
        "Cat",
        "Chicken",
        "Cow",
        "Cub",
        "Deer",
        "Dog",
        "Duck",
        "Eagle",
        "Elephant",
        "Frog",
        "Goat",
        "Gorilla",
        "Hamster",
        "Hippo",
        "Horse",
        "Kangaroo",
        "Koala",
        "Lion",
        "Monkey",
        "Mouse",
        "Octopus",
        "Ostrich",
        "Penguin",
        "Pig",
        "Rabbit",
        "Rhino",
        "Sheep",
        "Squirrel",
        "Tiger",
        "Wolf",
      ],
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

      if (this.hobby || this.personality || this.specie) {
        let params = this.filteredByName;

        if (this.specie) {
          params = params.filter((v) => {
            return v.species === self.specie;
          });
        }

        if (this.personality) {
          params = params.filter((v) => {
            return v.personality === self.personality;
          });
        }

        if (this.hobby) {
          params = params.filter((v) => {
            return v.nh_details.hobby === self.hobby;
          });
        }

        return params;
      }

      return this.filteredByName;
    },
    hasFilters() {
      return (
        this.nameFilter !== "" ||
        this.specie !== null ||
        this.personality !== null ||
        this.hobby !== null
      );
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
    clearFilters() {
      this.nameFilter = "";
      this.specie = null;
      this.personality = null;
      this.hobby = null;
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
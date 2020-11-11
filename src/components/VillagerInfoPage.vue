<template>
  <div v-if="villager">
    <div
      class="info-shade flex items-center justify-center"
      @click="$emit('close-info')"
    ></div>

    <div class="info-container flex items-center justify-center">
      <div class="info-modal w-4/5 bg-yellow-100 p-8 rounded-lg">
        <!-- TOP SECTION - GENERAL basic AND IMAGE -->
        <div class="flex">
          <div>
            <img :src="villager.image_url" class="w-24 mr-4" />
          </div>

          <div class="ml-4 w-full">
            <h2 class="text-4xl text-yellow-900">{{ villager.name }}</h2>

            <div
              class="flex items-center border-b-2 border-yellow-500 w-full pb-2"
            >
              <em class="text-xl text-yellow-800 font-black mr-4">{{
                villager.personality + " " + villager.species
              }}</em>
              <div
                class="bg-blue-200 px-3 rounded-full border-2 border-blue-600 text-blue-600 mr-2"
              >
                {{ villager.nh_details.hobby }}
              </div>
              <strong
                class="bg-yellow-600 text-yellow-200 border-2 border-yellow-700 px-4 rounded-full"
                >{{
                  villager.birthday_month + " " + villager.birthday_day
                }}</strong
              >
            </div>

            <div
              class="p-2 border-l-4 border-yellow-900 mt-2 italic text-yellow-800 text-xl bg-orange-200"
            >
              "{{ villager.quote }}"
            </div>
          </div>
        </div>

        <!-- BOTTOM SECTION - DETAILS -->
        <h3 class="text-2xl text-yellow-800 mt-4">Details</h3>
        <div class="bottom-section py-2 mt-2 text-yellow-900">
          <!-- INFO -->
          <div class="info-section">
            <div
              v-for="d in availableDetails"
              :key="d[0]"
              class="bg-yellow-300 mb-1 px-2 py-1 rounded-full"
            >
              <span class="font-bold uppercase">{{ d[0] }}:</span>
              {{ d[1] }}
            </div>
          </div>

          <!-- HOUSE INFO -->
          <div class="house-info-section p-4 bg-yellow-300 rounded-lg flex">
            <div
              class="flex flex-col items-center text-xl text-yellow-800 mb-2 p-2 w-64"
            >
              <h4>House Exterior</h4>
              <a :href="villager.nh_details.house_exterior_url" target="_blank"
                ><img :src="villager.nh_details.house_exterior_url"
              /></a>
            </div>
            <div
              class="flex flex-col items-center text-xl text-yellow-800 mb-2 p-2 w-64"
            >
              <h4>House Interior</h4>
              <a :href="villager.nh_details.house_interior_url" target="_blank"
                ><img :src="villager.nh_details.house_interior_url"
              /></a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "VillagerbasicPage",

  props: {
    villager: { type: Object, default: null },
  },

  data() {
    return {
      availableDetailList: [
        "id",
        "species",
        "personality",
        "gender",
        "sign",
        "clothing",
      ],
      availableNHDetailList: [
        "catchphrase",
        "hobby",
        "house_music",
        "house_wallpaper",
      ],
    };
  },

  watch: {
    villager(v) {
      if (v !== null) {
        document.querySelector("html").classList.add("menuOpen");
      } else {
        document.querySelector("html").classList.remove("menuOpen");
      }
    },
  },

  computed: {
    availableDetails() {
      let details = [];

      let basic = Object.entries(this.villager).filter((e) => {
        return this.availableDetailList.includes(e[0]);
      });

      let nh = Object.entries(this.villager.nh_details).filter((e) => {
        return this.availableNHDetailList.includes(e[0]);
      });

      basic.forEach((i) => details.push(i));
      nh.forEach((n) => details.push(n));

      return details;
    },
  },
};
</script>

<style lang="scss">
div.info-shade {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.6);
}

div.info-container {
  pointer-events: none;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;

  div.info-modal {
    pointer-events: all;

    box-shadow: 7px 7px 0px rgba(0, 0, 0, 0.1), 0 0 10px rgba(0, 0, 0, 0.2);

    div.bottom-section {
      max-height: 300px;
      overflow-y: scroll;
    }
  }
}

.menuOpen {
  overflow: hidden;
}
</style>
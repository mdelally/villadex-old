<template>
  <div v-if="villager">
    <div
      class="info-shade flex items-center justify-center"
      @click="$emit('close-info')"
    ></div>

    <div class="info-container flex items-center justify-center">
      <div class="info-modal w-4/5 bg-green-100 p-8 rounded-lg">
        <!-- TOP SECTION - GENERAL basic AND IMAGE -->
        <div class="flex">
          <div>
            <img :src="villager.image_url" class="w-24 mr-4" />
          </div>

          <div class="ml-4 w-full">
            <h2 class="text-4xl text-green-900">{{ villager.name }}</h2>

            <div
              class="flex items-center border-b-2 border-green-500 w-full pb-2"
            >
              <em class="text-xl text-green-800 font-black mr-4">{{
                villager.personality + " " + villager.species
              }}</em>
              <div
                class="bg-blue-200 px-3 rounded-full border-2 border-blue-600 text-blue-600 mr-2"
              >
                {{ villager.nh_details.hobby }}
              </div>
              <strong
                class="bg-green-600 text-green-200 border-2 border-green-700 px-4 rounded-full"
                >{{
                  villager.birthday_month + " " + villager.birthday_day
                }}</strong
              >
            </div>

            <div
              class="p-2 border-l-4 border-green-900 mt-2 italic text-green-800 text-xl bg-orange-200"
            >
              "{{ villager.quote }}"
            </div>
          </div>
        </div>

        <!-- BOTTOM SECTION - DETAILS -->
        <h3 class="text-2xl text-green-800 mt-4">Details</h3>
        <div class="bottom-section py-2 mt-2 text-green-900">
          <!-- INFO -->
          <div class="info-section">
            <div
              v-for="d in availableDetails"
              :key="d[0]"
              class="bg-green-300 mb-1 rounded-full flex"
            >
              <div
                class="detail-label rounded-full font-bold uppercase bg-green-700 text-green-200 px-2 py-1 min-w-1"
              >
                {{ detailLabel(d[0]) }}:
              </div>
              <div class="px-2 py-1">{{ parseDetail(d[1]) }}</div>
            </div>
          </div>

          <!-- HOUSE INFO -->
          <div class="house-info-section p-4 bg-green-300 rounded-lg flex">
            <div
              class="flex flex-col items-center text-xl text-green-800 mb-2 p-2 w-64"
            >
              <h4>House Exterior</h4>
              <a :href="villager.nh_details.house_exterior_url" target="_blank"
                ><img :src="villager.nh_details.house_exterior_url"
              /></a>
            </div>
            <div
              class="flex flex-col items-center text-xl text-green-800 mb-2 p-2 w-64"
            >
              <h4>House Interior</h4>
              <a :href="villager.nh_details.house_interior_url" target="_blank"
                ><img :src="villager.nh_details.house_interior_url"
              /></a>
            </div>
            <div
              class="flex flex-col items-center text-xl text-green-800 mb-2 p-2 w-64"
            >
              <h4>Villager Photo</h4>
              <a :href="villager.nh_details.photo_url" target="_blank"
                ><img :src="villager.nh_details.photo_url"
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
        "gender",
        "personality",
        "species",
        "sign",
        "clothing",
      ],
      availableNHDetailList: [
        "catchphrase",
        "hobby",
        "house_music",
        "house_wallpaper",
        "fav_colors",
        "fav_styles",
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

  methods: {
    detailLabel(label) {
      return label.replace("_", " ");
    },
    parseDetail(detail) {
      if (Array.isArray(detail)) {
        return detail.join(", ");
      }

      return detail;
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

    max-width: 1200px;

    box-shadow: 7px 7px 0px rgba(0, 0, 0, 0.1), 0 0 10px rgba(0, 0, 0, 0.2);

    div.bottom-section {
      max-height: 300px;
      overflow-y: scroll;
    }

    div.detail-label {
      flex-basis: 20%;
      border-top-right-radius: 0;
      border-bottom-right-radius: 0;
      text-align: center;
    }
  }
}

.menuOpen {
  overflow: hidden;
}
</style>
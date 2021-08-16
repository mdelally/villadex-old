<template>
  <Layout>
    <div class="bg-green-200 rounded-lg p-4 text-center text-green-900">
      <p class="mb-2">
        Quickly search and learn about
        <em>Animal Crossing: New Horizons®</em> villagers. This data is brought
        to you by the <a href="https://api.nookipedia.com/">Nookipedia API</a>.
      </p>

      <p class="font-bold text-xl mb-4">
        App built with love by Michael DeLally using VueJS/Gridsome and
        TailwindCSS.
      </p>

      <p class="text-xs">
        All characters, content, and imagery is copyright Nintendo.
      </p>

      <button
        class="
          hover:bg-green-800
          hover:text-green-200
          font-bold
          px-8
          py-2
          mt-4
          rounded-full
          hover:border-transparent
          border-2 border-green-800
          text-green-800
          bg-transparent
        "
        @click="clearData"
      >
        Clear and Update Villager Data
      </button>
    </div>

    <VillagerList @select-villager="setVillager" />

    <transition mode="out-in" name="info">
      <VillagerInfoPage
        v-if="currentVillager"
        :villager="currentVillager"
        @close-info="closeInfo"
      />
    </transition>
  </Layout>
</template>

<script>
import VillagerList from "@/components/VillagerList.vue";
import VillagerInfoPage from "@/components/VillagerInfoPage.vue";

export default {
  metaInfo: {
    title: "New Horizons Villager Codex",
  },

  components: { VillagerList, VillagerInfoPage },

  data() {
    return {
      currentVillager: null,
    };
  },

  methods: {
    setVillager(v) {
      this.currentVillager = v;
    },
    closeInfo() {
      this.currentVillager = null;
    },
    clearData() {
      if (confirm("Are you sure you want to clear and update?")) {
        localStorage.removeItem("villadex-villagers");
        window.location.reload();
      }
    },
  },
};
</script>

<style lang="scss">
.info-enter-active,
.info-leave-active {
  transition: all 0.35s ease-in-out;
}
.info-enter, .info-leave-to /* .cards-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>

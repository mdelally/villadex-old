<template>
  <Layout>
    <div class="bg-yellow-200 rounded-lg p-4 text-center text-yellow-900">
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
    </div>

    <VillagerList />
  </Layout>
</template>

<script>
import VillagerList from "@/components/VillagerList.vue";

export default {
  metaInfo: {
    title: "New Horizons Villager Codex",
  },

  components: { VillagerList },
};
</script>

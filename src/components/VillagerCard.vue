<template>
  <div class="villager-card-container p-2">
    <button
      class="w-full villager-card bg-yellow-100 rounded-lg p-4 flex flex-col justify-center items-center hover:bg-yellow-200 hover:shadow-lg border-4 border-transparent hover:border-yellow-700"
    >
      <h3
        class="px-4 py-2 rounded-full shadow text-xl mb-2"
        :style="villagerName(v.id)"
      >
        {{ v.name }}
      </h3>
      <img class="h-32" :src="v.image_url" :alt="v.name" />
      <em class="text-yellow-900 mb-2">{{
        v.personality + " " + v.species
      }}</em>
      <div
        class="bg-blue-200 px-3 py-1 rounded-full mb-1 border-2 border-blue-600 text-blue-600"
      >
        {{ v.nh_details.hobby }}
      </div>
      <strong
        class="bg-yellow-600 text-yellow-200 border-2 border-yellow-700 px-4 py-1 rounded-full"
        >{{ v.birthday_month + " " + v.birthday_day }}</strong
      >
    </button>
  </div>
</template>

<script>
export default {
  name: "VillagerCard",

  props: {
    v: { type: Object, default: () => {} },
  },

  methods: {
    villagerName(id) {
      return {
        color: "#" + this.v.text_color,
        backgroundColor: "#" + this.v.title_color,
      };
    },
  },
};
</script>

<style lang="scss">
button.villager-card {
  z-index: 1;

  transition: all 100ms ease-in-out;

  &:hover {
    transform: translateY(-1.5em) translateZ(3px) scale(1.1) rotate(-3deg);
  }

  h3 {
    border-radius: 30px 52% 52% 48%/32px 58% 34px 68%;
  }
}
</style>
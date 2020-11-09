<template>
  <div class="villager-card-container p-2">
    <button
      class="w-full villager-card bg-yellow-100 rounded-lg p-4 flex flex-col justify-center items-center hover:bg-yellow-200 duration-100 transform hover:scale-110 hover:shadow-lg hover:-rotate-3 border-2 border-transparent hover:border-yellow-600"
    >
      <h3
        class="px-4 py-1 rounded-full shadow text-xl mb-2"
        :style="villagerName(v.id)"
      >
        {{ v.name }}
      </h3>
      <img class="h-32" :src="v.image_url" :alt="v.name" />
      <em class="text-yellow-900">{{ v.personality + " " + v.species }}</em>
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